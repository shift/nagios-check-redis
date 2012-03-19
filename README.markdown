# Nagios Redis Check

commands.cfg:

    define command {
      command_name check_redis
      command_line /path_to_nagios_checks/check_redis.rb $ARG1$ -H $HOSTADDRESS$ -p $ARG2$ -P $ARG3$ -t $ARG4$ -w $ARG5$ -c $ARG6$
    }

services.cfg:

    define service {
      check_command  check_redis!used_memory!6379!password!5!100000!1048576!2097152
      ...
    }

