# Nagios Redis Check

commands.cfg:

    define command {
      command_name check_redis
      command_line /path_to_nagios_checks/check_redis.rb $ARG1$ -H $HOSTADDRESS$ -p $ARG2$ -P $ARG3$ -t $ARG4$ -w $ARG5$ -c $ARG6$
    }

    define command {
      command_name check_redis_key
      command_line /path_to_nagios_checks/check_redis.rb $ARG1$ -K -D $ARG7$ -H $HOSTADDRESS$ -p $ARG2$ -P $ARG3$ -t $ARG4$ -w $ARG5$ -c $ARG6$
    }

    define command {
      command_name check_redis_and_graph
      command_line /path_to_nagios_checks/check_redis.rb $ARG1$ -H $HOSTADDRESS$ -p $ARG2$ -P $ARG3$ -t $ARG4$ -w $ARG5$ -c $ARG6$ -G $ARG7$ -O $ARG8$ -M $ARG9$
    }

    define command {
      command_name check_redis_and_graph_elapsed_time
      command_line /path_to_nagios_checks/check_redis.rb $ARG1$ -H $HOSTADDRESS$ -p $ARG2$ -P $ARG3$ -t $ARG4$ -w $ARG5$ -c $ARG6$ -G $ARG7$ -O $ARG8$ -M $ARG9$ -R
    }

services.cfg:

    define service {
      check_command  check_redis!used_memory!6379!password!5!100000!1048576!2097152
      ...
    }

    define service {
      check_command  check_redis_key!key!6379!password!5!100000!1048576!2097152!0
      ...
    }

    define service {
      check_command  check_redis_and_graph!used_memory!6379!password!5!100000!1048576!2097152!graphite.example.com!2003!system.foo
      ...
    }

    define service {
      check_command  check_redis_and_graph_elapsed_time!used_memory!6379!password!5!100000!1048576!2097152!graphite.example.com!2003!system.foo
      ...
    }
