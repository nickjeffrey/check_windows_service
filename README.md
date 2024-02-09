# check_windows_service
nagios check for Windows services using SNMP

# Requirements
perl, snmpget, snmpwalk on nagios server, SNMP service enabled on monitored Microsoft Windows hosts

# Configuration

You will need a section in the services.cfg file on the nagios server that looks similar to the following.
```
# Parameters are SNMP community name and "Name of Service" 
define service {
        use                             generic-service
        host_name                       winserv1,winserv2
        service_description             service Print Spooler
        check_command                   check_windows_service!public!"Print Spooler"
        }
```

You will need a section in the commands.cfg file on the nagios server that looks similar to the following.
```
# 'check_windows_service' command definition for monitoring Microsoft Windows services via SNMP
# parameters are -H hostname -C snmp_community --servicename="My Service Name"
define command{
        command_name    check_windows_service
        command_line    $USER1$/check_windows_service -H $HOSTADDRESS$ -C $ARG1$ --servicename=$ARG2$
        }
```

On the monitored Microsoft Windows host, ensure the SNMP service is installed and running

<img src=images/snmp_service.png>

On the monitored Microsoft Windows host, ensure the SNMP service has a known community string and allows incoming connections from at least the nagios server.

<img src=images/snmp_security.png>

# Output

You will see output similar to one of the following messages:
```
service OK - Print Spooler is running
service WARN - BogusServiceName was not found, please verify BogusServiceName is a valid service name 
service WARN - sysDescr indicates this is not a Windows machine, so cannot be checked for Windows Services: Linux ns1 5.14.0-350.el9.x86_64 #1 SMP PREEMPT_DYNAMIC
```
