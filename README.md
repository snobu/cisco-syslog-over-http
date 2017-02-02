# cisco-syslog-over-http
Send Syslog messages over HTTP from Cisco EEM TCL

### Event Manager Applet configuration

```cisco
CISCO-1811#sh run | s event
event manager directory user policy "flash:/"
event manager directory user library "flash:/"
event manager directory user repository tftp://1.1.1.3/
event manager applet TRIGGER_ON_SYSLOG
 event syslog occurs 1 pattern "%.*"
 action 1.0 string trimleft "$_syslog_msg"
 action 2.0 cli command "enable"
 action 2.1 cli command "tclsh flash:/sendevent.tcl \"$_string_result\""
```

Ref:
https://www.dslreports.com/forum/r31226025-Cisco-EEM-pass-Syslog-message-to-TCL-script
