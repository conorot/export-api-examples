BETA
==========
###****This endpoint is currently in beta. It is subject to change without notice.****




Purpose
------------
This endpoint will allow you to get bulk payment and order data from merchants that have installed your app. This service is not real-time, but will significantly reduce your chances of hitting an API limit restriction. Developers that require order and payment histories on merchants should integrate this service into their data collecting implementation. 


Usage Notes
------------
-Availability of the service:
UTC: MON-FRI 08:00-12:00, SAT 09:00-12:00, SUN 09:00-13:00, otherwise we return a 503 with availability hours. 
Sandbox does not have this limitation.


-1000 objects per file returned. If your response is more than 1000 objects we'll return an array of files to capture.


-Each export file is available for 24hrs after which we'll delete it.


-The max time range is 30 days. So if you want spans of data longer than that, you'll need to send a few requests.




How to use
------------
You will do the POST call to /v3/merchants/{merchant_Id}/exports with the appropriate payload.
The payload includes:
  export_type (Orders or Payments)
  start_time (in UTC)
  end_time (in UTC)
  ```
    {
        'type': export_type,
        'startTime': start_time,
        'endTime': end_time,
    }
  ```


The response of the exports object will include the export id for you to use. 
```
{
  'status': 'PENDING', 
  'modifiedTime': modified_time, 
  'merchantRef': 
    {'id': 'merchant_id'},
  'percentComplete': 0, 
  'startTime': start_time, 
  'createdTime': created_time, 
  'endTime': end_time, 
  'type': 'ORDERS', 
  'id': 'export_id'
}
```


Once the call has been made, you will occasionally do a GET call to /v3/merchants/{merchant_Id}/exports/{export_Id} to check the status and percent complete.


STATUS: 
PENDING - In queue to be processed  
IN_PROGRESS - Being processed  
COMPLETE - Process complete


```
{
  'status': 'PENDING', 
  'modifiedTime': modified_time, 
  'merchantRef': 
    {'id': 'merchant_id'},
  'percentComplete': 0, 
  'startTime': start_time, 
  'createdTime': created_time, 
  'endTime': end_time, 
  'type': 'ORDERS', 
  'id': 'export_id'
}
```


Once it is complete your export object will include urls to the files for you to download the export.


```
{
  'status': 'COMPLETE', 
  'modifiedTime': modified_time, 
  'merchantRef': 
    {'id': 'merchant_id'},
  'percentComplete': 100, 
  'startTime': start_time, 
  'createdTime': created_time, 
  'endTime': end_time, 
  'type': 'ORDERS', 
  'id': 'export_id',
  'exportUrls': 
    {'elements':
      {'url': URL}
    }
}
```




