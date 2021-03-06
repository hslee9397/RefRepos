Before running the steps check the OCP version it should be openshift v3.9.33 or above. 



 ## Steps : 
 
```

oc project default
oc adm router --replicas=0 
oc set env dc/router ROUTER_ALLOW_WILDCARD_ROUTES=true
oc scale dc/router --replicas=1
oc project <3scale project> 
oc export route apicast-wildcard-router -o yaml > wildcard-router.yml
           make following change spec : 
            a. add wildcard in front of the existing host value like : 
	    host: apicast-wildcard.<teneant(or)env>.${WILDCARD_DOMAIN}
	    ex : apicast-wildcard.uat.13.251.251.251.nip.io
           b. change the wildcardPolicy like :
	      wildcardPolicy: Subdomain

        oc delete -f wildcard-router.yml
       oc apply -f wildcard-router.yml
````	
If you see the <3scale project>  routes, you can observe something * added in the hostname. 

Sample Wildcrd route :
![alt text](https://github.com/mohansidda/RefRepos/blob/master/3scale/wildcard-route.png "Wildcardroute ")


Once you configure the wildcard router to accept *.apicast-wildcard.13.251.251.251.nip.io
you don't need to create any routes for new 3Sacale services. However, you need to keep your staging and production URL's similar to this : 

https://api-3scale-apicast-staging.uat.13.251.251.251.nip.io:443

https://api-3scale-apicast-production.uat.13.251.251.251.nip.io:443

Sample 3Scale - Staging and Production Base URL's : 
![alt text](https://github.com/mohansidda/RefRepos/blob/master/3scale/staging-productionURL.png "Sample URL's ")


This means. you can keep the service name and environment in * place after that *.<wildcard domain > has to be standard and consistent for all the new services. 


