## Postmortem
This is the postmortem for the outage that struck the whole infrastructure
## 504 Error while accessing the landing page
up to 70% of users were affected at first but then the dashboard showed that traffic was delivered to few nodes
over somewhat sparse periods
it started 9:37pm(GMT+1) and ended on the same day 11:51pm(GMT+1)

#### outage summary on 20th june 2022(GMT+1):
as the app has gaiend traction the server instances simlpy couldn't manage the traffic
so the complaints starte pouring on twitter and the tech lead noticed quickly 
 
 we thought at first that maybe this is a ddos attack but the SRE engineers confirmed otherwise
## Timeline(GMT+1):
the responsible members here are the two senior SRE engineers and the tech lead

9:37pm - internal sserver error for users

9:41pm - making a new branch with the time as name for testing adding django instances

9:44pm - testing works and tests work for it

9:50 - problem persists

9:54 - checking nginx config for modifications

11:51pm - problem resolved after updating  nginx load balancing


## Root cause:
few django instances were operating so we made containers of the app after checking the server capabilities
and making a somewhat optimistic projection of the user count and we concluded that for the moment we can manage

## Remediation / Future Work:
* Audit our regular expressions and post validation workflow for any similar issues

This seems like a solid follow up task.

* Add controls to our load balancer to disable the healthcheck – as we believe everything but the home page would have been accessible if it wasn’t for the the health check

This was clarified as a control that an operator could use to disable the health checks if the same issue reoccurred.  It does not solve the larger fleet utilization issue but it would have prevented this issue from reoccuring in which all the hosts were marked as unhealthy on the load balancer.  Despite other sites possibly being slow from the utilization, they still would have been "up".

## impact:

the outage showed us the  value of caching content and we should consider using more client side caching
but the outage was widely noticed  and we apologized for it

## Prevention:
the app must scale with correlation to the number of users 

## Citations / Additional reading

