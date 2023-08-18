# check_http_power
Nagios plugin to check power supply in Belgium via (https://economie.fgov.be/nl/themas/energie/bevoorradingszekerheid/elektriciteit/kans-op/de-actuele-situatie-op-het)https://economie.fgov.be/nl/themas/energie/bevoorradingszekerheid/elektriciteit/kans-op/de-actuele-situatie-op-het

Data comes from https://extcom.azurewebsites.net/api/networkstate
This is an XML-file containing the status of the next seven days. The file is parsed and the most severe state is reported in Nagios.
* Normale situatie: OK
* Risico op stroomtekort: Warning
* Risico op afschakeling: Warning
* Afschakeling aangekondigd: Critical

# use in Nagios

## define command
    define command{
    command_name check_http_power
    command_line $USER1$/check_http_power 
    }
  
## define servcice

    define service {
	      use 			generic-service,graphed-service	;
	      host_name 		Power;
	      service_description 	Stroomvoorziening;
	      check_command 		check_http_power;
  	    check_interval		360;
	      check_period		powercheck;
	      retry_interval		3;
	      max_check_attempts	3;
	      notification_interval	360;
	      notification_period	powercheck;
	      notification_options	w,c,u;
	      contact_groups		admins;

       
