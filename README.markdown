# Nagios Redis Check

Still in its early stages, currently in use in a production enviroment.

define command{
    command_name check_redis
    command_line $USER1$/check_redis -H $HOSTADDRESS$
  }
