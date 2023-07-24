vrrp_script check_haproxy {
   script "sh /usr/local/bin/haproxy-service-check.sh"
   interval 2
   fall 2
   rise 2
   weight 2
}

global_defs {
   notification_email {
      {{ keepalived.email_notif }}
   }
}

vrrp_instance LB1 {
   state MASTER
   interface {{ keepalived.interface }}
   virtual_router_id {{ keepalived.virtual_router_id_1 }}
   priority {{ keepalived.priority1_1 }}
   advert_int {{ keepalived.advert_int }}
   accept

   authentication {
      auth_type PASS
      auth_pass {{ keepalived_auth_pass }}
   }

   virtual_ipaddress {
      {{ keepalived.vip1_1 }} dev {{ keepalived.interface }}
      {{ keepalived.vip1_2 }} dev {{ keepalived.interface }}
   }

   track_script {
      check_haproxy
   }
   
   track_interface {
      {{ keepalived.interface }}
   }

}

vrrp_instance LB2 {
   state BACKUP
   interface {{ keepalived.interface }}
   virtual_router_id {{ keepalived.virtual_router_id_2 }}
   priority {{ keepalived.priority2_1 }}
   advert_int {{ keepalived.advert_int }}
   accept

   authentication {
      auth_type PASS
      auth_pass {{ keepalived_auth_pass }}
   }

   virtual_ipaddress {
      {{ keepalived.vip2_1 }} dev {{ keepalived.interface }}
      {{ keepalived.vip2_2 }} dev {{ keepalived.interface }}
   }

   track_script {
      check_haproxy
   }
   
   track_interface {
      {{ keepalived.interface }}
   }

}
