```
global_defs {
   enable_script_security
   script_user root
}

vrrp_script check_haproxy {
   script "killall -0 haproxy"
   interval 2
   weight 2
}

vrrp_instance KUBE_API_LB {
   state MASTER
   interface ens33
   virtual_router_id 51
   priority 101
   # The virtual ip address shared between the two loadbalancers
   virtual_ipaddress {
      192.168.15.130/32
   }
   track_script {
      check_haproxy
   }
}
```
