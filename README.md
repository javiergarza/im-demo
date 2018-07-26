# im-demo
Some examples showing how images are automaticommandy transformed on [developer.akamai.com](https://developer.akamai.com/) using Akamai's Image Manager.
 
## How to use the sample files

The sample files contain Image Manager policies in JSON format which you can just download and activate using the [Image Manager OPEN API](https://developer.akamai.com/api/luna/imaging/overview.html).

In order to interact with the Image Manager API, you need a set of OPEN API credentials with READ-WRITE access to the [Image Manager OPEN API](https://developer.akamai.com/api/luna/imaging/overview.html) (see the [Getting Started](https://developer.akamai.com/introduction/) documentation for information on setup API credentials). 

You also need to know the name of the Image Manager Policy Set (also known as "API Key" within the Property Manager's Image Manager behavior). For example in the property manager configuration for [developer.akamai.com](https://developer.akamai.com/), the Image Manager Policy Set is commanded "developer_akamai_com-4903103"; therefore all the Image Manager API commands that want to interact with that specific policy set will need to include the following HTTP Header: `Luna-Token:developer_akamai_com-4903103`

## Screenshots
* Property Manager: The Property Manager behavior on the Akamai Property Manager for developer.akamai.com looks like this: [Image Manager Property Manager Behavior on developer.akamai.com](images/DAC-IM-behavior.png)
* Image Manager Policy Manager: The Policy Manager for developer.akamai.com looks like this: [Image Manager Property Manager Behavior on developer.akamai.com](images/DAC-IM-policy-manager.png)

## Sample JSON IM policy Files
### [dacimg_crop.json](json/dacimg_crop.json)


### [dacimg_filter.json](json/dacimg_filter.json)

### [dacimg_resize.json](json/dacimg_resize.json)

### [dacimg_watermark.json](json/dacimg_watermark.json)

### [dacimg_conditional-letterbox.json](json/dacimg_conditional-letterbox.json)

### [dacimg_conditional-orientation.json](json/dacimg_conditional-orientation.json)

## Image Manager sample API commands

In the examples below we use HTTPie (http), and it's edgegrid extension: a very useful CLI tool to explore APIs. Another tool mentioned below is jq, which is another CLI tool that will help us filter and format JSON. See [this Blog](http://bit.ly/2Odo1lA) for instructions on how to install and use HTTPie and jq
 
### List all Image Manager policies configured on the Policy Set "developer_akamai_com-4903103" (replace that with the right name for your policy set)
```
http --auth-type=edgegrid -a dac-imaging: :/imaging/v2/policies Luna-Token:developer_akamai_com-4903103
```  
Click [here](json/dacimg_ALL-policies.json) to see the OUTPUT of the above command

### Let's add the "session" parameterm so we can simplify commands going forward
```
http --session=dacimg --auth-type=edgegrid -a dac-imaging: :/imaging/v2/policies Luna-Token:developer_akamai_com-4903103
```  
Click [here](json/dacimg_ALL-policies.json) to see the OUTPUT of the above command

### Voila! We can now reference the session "dacimg" going forward and save some typing. Here is the same command as before: 
```
http --session=dacimg :/imaging/v2/policies 
```  
Click [here](json/dacimg_ALL-policies.json) to see the OUTPUT of the above command

### Save all the policies into a file (in our example the file is commanded `dacimg_ALL-policies.json`)
```
http --session=dacimg :/imaging/v2/policies > dacimg_ALL-policies.json
```
### Filter the policy names 
```
http --session=dacimg :/imaging/v2/policies | jq .items[].id 
```
Click [here](json/dacimg_quoted-policy-names.txt) to see the OUTPUT of the above command

### Filter the policy names and remove the double quotes 
```
http --session=dacimg :/imaging/v2/policies | jq .items[].id | sed 's/"//g'`
```
Click [here](json/dacimg_policy-names.txt) to see the OUTPUT of the above command

### Loop through all the policy names and save each of them on a separate json file
This is the command I ran to generate all the json files I am including on this repository
```
for policy in `http --session=dacimg :/imaging/v2/policies | jq .items[].id | sed 's/"//g'`; do http --session=dacimg :/imaging/v2/policies/${policy} | jq . > dacimg_${policy}.json ; done
```

### Create a clone of the `conditional-orientation` policy called `conditional-orientation2` which rotates portrait images 90 degrees
```
sed 's/landscape/portrait/' dacimg_conditional-orientation.json > dacimg_conditional-orientation2.json
```
Click [here](json/dacimg_conditional-orientation2.json) to see the OUTPUT of the above command

### Submit a new policy called conditional-orientation2 out of the json generated above
```
http --session=dacimg PUT :/imaging/v2/policies/conditional-orientation2 < dacimg_conditional-orientation2.json
```
Click [here](json/dacimg_conditional-orientation2_created.json) to see the OUTPUT of the above command

### List all the policies and ensure the new policy is there
```
http --session=dacimg :/imaging/v2/policies | jq .items[].id
```
Click [here](json/dacimg_quoted-policy-names2.txt) to see the OUTPUT of the above command

### Delete the newly created policy called conditional-orientation2
```
http --session=dacimg DELETE :/imaging/v2/policies/conditional-orientation2 
```
Click [here](json/dacimg_conditional-orientation2_deleted.json) to see the OUTPUT of the above command

