
# Step 1:  execute this to configure debizuim for postgres database
curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" 127.0.0.1:8083/connectors/ --data "@debezium.json"

if you get error other than sucess code 200 , some thing wroing on your connector plug in config.

# step : 2 to test connector plug-ins  are configured 
curl -s http://localhost:8083/connector-plugins | jq


