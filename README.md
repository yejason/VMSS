# VMSS
create azure vmss with existing VHD and setup auto scale with CPU, and set NAT rules for backend, and set load balancer rules for port 80  (IIS)



    
1) Auto scale this VMss with CPU usage:
```
"rules": [
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', variables('namingInfix'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 60.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT1M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', variables('namingInfix'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 30.0
                },
```                
  2) Use VHD uri to create VMSS:
  ```
  "sourceImageVhdUri": {
			"type": "string",
			"metadata": {
				"description": "The source of the blob containing the custom image"
			}
		},
  "storageProfile": {
            "osDisk": {
			 "name": "vmssosdisk",
             "createOption": "FromImage",
			 "osType": "Windows",
             "image": {
			  "uri": "[parameters('sourceImageVhdUri')]"
			 }
			}
          },      

                
