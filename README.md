# cisco-syslog-over-http
Send Syslog messages over HTTP from Cisco EEM TCL

### Event Manager Applet configuration

IOS Version: 15.1(3)T1
```ios
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

### Expected result

```http
POST / HTTP/1.1
Accept: */*
Host: some.http.destination
User-Agent: Snobu Speshul TCL HTTP/1.1 Client library // build 21
Connection: close
Content-Length: 151
Content-Type: application/json

{
    "RouterTimestamp": "2017-02-02T10:09:49",
    "Message": "*Feb  2 10:09:49.307: %CLEAR-5-COUNTERS: Clear counter on all interfaces by console"
}
```

Ref:
https://www.dslreports.com/forum/r31226025-Cisco-EEM-pass-Syslog-message-to-TCL-script
