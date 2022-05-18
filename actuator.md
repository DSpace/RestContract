# Actuator
Actuator endpoints let you monitor and interact with DSpace. By default the following endpoints are enabled:

**/actuator/info**: Displays arbitrary application info. Only administrators can access this endpoint. 
To modify the data exposed by the info endpoint, you can add properties with info prefix to the actuator.cfg configuration file.

**/actuator/health**: Shows application health information. The status is calculated based on the aggregation of the status of the configured components; only the administrator can see the detail of the components. 
To see the status of a single component, contact the endpoint /actuator/health/{component}.
The statuses that the application can have are:
* UP: the application is up and running without issues.
* UP_WITH_ISSUES: the application is up and running non-blocking issues. 
* OUT_OF_SERVICE: the application has critical issues.
* DOWN: the application has fatal issues.

For more information on how to configure the previously described endpoints and how to enable others, see [here](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html#actuator.enabling).
