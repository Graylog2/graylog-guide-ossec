# How to send OSSEC alerts into Graylog

This guide describes how to send structured OSSEC alert logs into Graylog.

![](https://s3.amazonaws.com/graylog2public/ossec_integration/dashboard.png)

## Configuring Graylog

### Installing the CEF input plugin

We’ll use the CEF format to forward logs from OSSEC to Graylog. There is an official Graylog plugin to read OSSEC logs via TCP or UDP and you can get it here:

* [Graylog CEF message input](https://github.com/Graylog2/graylog-plugin-cef)

Follow the installation instructions in the linked repository.

### Setting up a CEF input 

After installing the CEF input plugin and restarting `graylog-server`, go to `System -> Inputs` in your Graylog Web Interface and start a new **CEF UDP Input**.

Remember the port you configure it to listen on. We’ll need it in the next step.
  
## Configuring OSSEC

After starting the CEF UDP input in the previous step, open your OSSEC configuration file (usually located at `/var/ossec/etc/ossec.conf`) and add the following to the global `<ossec_config>` path:

    <syslog_output>
        <server>graylog.example.org</server>
        <port>12000</port>
        <format>cef</format>
    </syslog_output>

Make sure to configure the same port you have your Graylog UDP CEF input listen on.

You also need to enable the OSSEC syslog subsystem on most operating systems. On Linux, execute this command:

    /var/ossec/bin/ossec-control enable client-syslog

Restart OSSEC and see if messages arrive.

    /var/ossec/bin/ossec-control restart

## Result

Now all OSSEC alerts should arrive in Graylog with nicely parsed and extracted fields:

![](https://s3.amazonaws.com/graylog2public/ossec_integration/message.png)
