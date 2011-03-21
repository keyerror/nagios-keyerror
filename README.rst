keyerror.com Nagios client
~~~~~~~~~~~~~~~~~~~~~~~~~~

This is the Nagios client for the `KeyError <https://keyerror.com/>`_ error
service.

It checks that current error rate is below a certain threshold. The time
interval over which the rate is calculated is configurable.

Installation
------------

1. Copy or otherwise install the ``check_keyerror`` script to a location
   accessible by your Nagios installation.

2. Ensure ``check_keyerror`` is executable::

   $ chmod +x /path/to/check_keyerror

3. If you are using Python 2.5 or earlier, install *python-simplejson*. On
   Debian GNU/Linux this can be achieved using::

   $ apt-get install python-simplejson

4. Check if installation was successful::

   $ /path/to/check_keyerror

  The script should print::

    CRITICAL: usage: <api_key> <interval> <warning> <critical>

5. Add a Nagios **command** stanza for this check::

    define command {
         command_name    check_keyerror
         command_line    /path/to/check_keyerror $ARG1$ $ARG2$ $ARG3$ $ARG4$
    }

6. Add a Nagios **service** stanza for each project you wish to monitor::

    define service {
      <host_name|hostgroup_name>    <$host_name|$hostgroup_name>
      service_description           Error rate
      check_command                 check_keyerror!API_KEY!WARNING
    }

   <host_name/hostgroup>
     All checks in Nagios are associated with a particular host or hostgroup so
     you must decide where to attach this check. If no host is particularly
     suitable, you could create a virtual host or hostgroup to "own" the
     check.You should not attach it to more than one server as that will result
     in duplicated messages. See the `Nagios documentation
     <http://nagios.sourceforge.net/docs/3_0/objectdefinitions.html#service>`_
     for more details.

   API_KEY
     Your KeyError API key. This can be obtained via the "Settings" menu option
     from your dashboard.

   WARNING
     Warning error rate threshold; if the number of errors within **INTERVAL**
     seconds reaches or exceeds this value, a Nagios "WARNING" event will be
     fired.

   CRITICAL
     Critical error rate threshold; if the number of errors within **INTERVAL**
     seconds reaches or exceeds this value, a Nagios "CRITICAL" event will be
     fired.

   INTERVAL
     The number of seconds of history we will be concerned with.
