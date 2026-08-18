# Multi-Factor Authentication

[Back to the list of all defined endpoints](endpoints.md)

TOTP-based Multi-Factor Authentication (MFA) adds an optional second factor to user authentication. When enabled, a user must verify a time-based one-time password (TOTP) from an authenticator app after their initial login before gaining full API access.

## Configuration

Three configuration properties control MFA behavior:

| Property | Default | Description |
|----------|---------|-------------|
| `mfa.totp.enabled` | `true` | Global kill switch. When false, all MFA endpoints are unavailable and MFA verification is skipped. |
| `mfa.totp.mandatory` | `false` | When true, all users MUST enroll in MFA. Unenrolled users receive restricted tokens until they complete setup. |
| `mfa.totp.issuer` | `DSpace` | Display name shown in authenticator apps (e.g. Google Authenticator). |

## Two-Phase Login

When a user has MFA enabled, the login flow becomes two-phase:

1. **Phase 1**: Normal login via `POST /api/authn/login` succeeds but the JWT contains `mfa_verified: false`.
2. **Phase 2**: The user calls `POST /api/authn/mfa/verify` with a valid TOTP code. A new JWT with `mfa_verified: true` is returned.

While `mfa_verified` is `false`, all endpoints return `403 Forbidden` with the following body:

```json
{
  "error": "MFA verification required",
  "mfa_required": true
}
```

Exempt endpoints (accessible with `mfa_verified: false`):
- `GET /api/authn/mfa/status`
- `POST /api/authn/mfa/setup`
- `POST /api/authn/mfa/verify-setup`
- `POST /api/authn/mfa/verify`
- `POST /api/authn/logout`
- `GET /api/authn/status`

## JWT Claim

A new claim `mfa_verified` (boolean) is added to all JWTs:

| claim | description |
|-------|-------------|
| mfa_verified | `true` if MFA verification is complete or not required; `false` if the user must still verify |

## Status

**GET /api/authn/mfa/status**

Returns the current user's MFA enrollment status. Requires authentication.

```json
{
  "enabled": false,
  "globallyEnabled": true,
  "mandatory": false,
  "setupRequired": false,
  "remainingRecoveryCodes": 0
}
```

Fields:
- `enabled`: whether the user has MFA enrolled and active
- `globallyEnabled`: whether MFA is enabled system-wide
- `mandatory`: whether MFA enrollment is required for all users
- `setupRequired`: true when MFA is mandatory but the user has not enrolled
- `remainingRecoveryCodes`: number of unused recovery codes remaining

Return codes:
- 200 OK
- 401 Unauthorized if not authenticated

## Setup

**POST /api/authn/mfa/setup**

Initiates MFA enrollment by generating a new TOTP secret. Requires authentication. Can be called multiple times before verification (replaces the pending secret each time).

```json
{
  "secret": "JBSWY3DPEHPK3PXP...",
  "provisioningUri": "otpauth://totp/DSpace:user@example.com?secret=JBSWY3DPEHPK3PXP...&issuer=DSpace&algorithm=SHA1&digits=6&period=30"
}
```

The `provisioningUri` is a standard `otpauth://` URI suitable for QR code generation.

Return codes:
- 200 OK
- 401 Unauthorized if not authenticated
- 409 Conflict if MFA is already enabled for this user

## Verify Setup

**POST /api/authn/mfa/verify-setup**

Confirms MFA enrollment by verifying the first TOTP code from the user's authenticator app. This enables MFA and generates recovery codes.

Request body (JSON):
```json
{
  "code": "123456"
}
```

Response (200 OK):
```json
{
  "recoveryCodes": [
    "a1b2c3d4",
    "e5f6g7h8",
    "i9j0k1l2",
    "m3n4o5p6",
    "q7r8s9t0",
    "u1v2w3x4",
    "y5z6a7b8",
    "c9d0e1f2"
  ]
}
```

Recovery codes are shown once and cannot be retrieved again. Each code is single-use.

Return codes:
- 200 OK with recovery codes
- 400 Bad Request if code is invalid
- 401 Unauthorized if not authenticated
- 404 Not Found if setup was not initiated

## Verify (Login)

**POST /api/authn/mfa/verify**

Verifies a TOTP code or recovery code during the two-phase login flow. On success, a new JWT with `mfa_verified: true` is returned in the `Authorization` response header.

Request body (JSON):
```json
{
  "code": "123456"
}
```

A recovery code can be used in place of a TOTP code. Recovery codes are case-insensitive and single-use.

Return codes:
- 200 OK. New JWT in `Authorization` header.
- 400 Bad Request if code is invalid or expired
- 401 Unauthorized if not authenticated

## Disable

**POST /api/authn/mfa/disable**

Disables MFA for the current user. Requires a valid TOTP code for confirmation.

Request body (JSON):
```json
{
  "code": "123456"
}
```

Return codes:
- 204 No Content on success
- 400 Bad Request if code is invalid
- 401 Unauthorized if not authenticated
- 404 Not Found if MFA is not enabled

## Recovery Codes

**POST /api/authn/mfa/recovery-codes**

Regenerates recovery codes for the current user. Requires a valid TOTP code for confirmation. Previous codes are invalidated.

Request body (JSON):
```json
{
  "code": "123456"
}
```

Response (200 OK):
```json
{
  "recoveryCodes": [
    "a1b2c3d4",
    "e5f6g7h8",
    "i9j0k1l2",
    "m3n4o5p6",
    "q7r8s9t0",
    "u1v2w3x4",
    "y5z6a7b8",
    "c9d0e1f2"
  ]
}
```

Return codes:
- 200 OK with new recovery codes
- 400 Bad Request if TOTP code is invalid
- 401 Unauthorized if not authenticated
- 404 Not Found if MFA is not enabled

## Admin Endpoints

### Admin Status

**GET /api/authn/mfa/admin/{uuid}/status**

Returns MFA status for any user. Requires admin privileges.

Response is the same as the regular status endpoint.

Return codes:
- 200 OK
- 401 Unauthorized if not authenticated
- 403 Forbidden if not an admin
- 404 Not Found if user does not exist

### Admin Disable

**POST /api/authn/mfa/admin/{uuid}/disable**

Force-disables MFA for any user without requiring a TOTP code. Invalidates all existing sessions for that user. Requires admin privileges.

No request body required.

Return codes:
- 204 No Content on success
- 401 Unauthorized if not authenticated
- 403 Forbidden if not an admin
- 404 Not Found if user does not exist or MFA is not enabled
