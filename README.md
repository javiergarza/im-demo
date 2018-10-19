# im-demo
Some examples showing how images are automaticommandy transformed on [developer.akamai.com](https://developer.akamai.com/) using Akamai's Image Manager.
 
## How to use the sample files

The sample files contain Image Manager policies in JSON format which you can just download and activate using the [Image Manager OPEN API](https://developer.akamai.com/api/luna/imaging/overview.html).

In order to interact with the Image Manager API, you need a set of OPEN API credentials with READ-WRITE access to the [Image Manager OPEN API](https://developer.akamai.com/api/luna/imaging/overview.html) (see the [Getting Started](https://developer.akamai.com/introduction/) documentation for information on setup API credentials). 

You also need to know the name of the Image Manager Policy Set (also known as "API Key" within the Property Manager's Image Manager behavior). For example in the property manager configuration for [developer.akamai.com](https://developer.akamai.com/), the Image Manager Policy Set is commanded "developer_akamai_com-4903103"; therefore all the Image Manager API commands that want to interact with that specific policy set will need to include the following HTTP Header: `Luna-Token:developer_akamai_com-4903103`

## Screenshots
* Property Manager: The Property Manager behavior on the Akamai Property Manager for developer.akamai.com looks like this: [Image Manager Property Manager Behavior on developer.akamai.com](images/DAC-IM-behavior.png)
* Image Manager Policy Manager: The Policy Manager for developer.akamai.com looks like this: [Image Manager Property Manager Behavior on developer.akamai.com](images/DAC-IM-policy-manager.png)

## Example URLs

### Resize using manual hints 
* Pristine: https://developer.akamai.com/image-manager/img/gallery-1.jpg
* Optimized: https://developer.akamai.com/image-manager/img/gallery-1.jpg?imwidth=500 

### Auto-crop to face and render in black and white 
* Pristine: https://developer.akamai.com/image-manager/img/gallery-1.jpg
* Optimized: https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=filter

### Add watermark
* Pristine: https://developer.akamai.com/image-manager/img/gallery-1.jpg
* Optimized: https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=watermark

### Create letterbox effect
* Pristine: https://developer.akamai.com/image-manager/img/gallery-1.jpg
* Optimized: https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=conditional-letterbox

### Rotate 90 degrees images in landscape mode
* Pristine in portrait: https://developer.akamai.com/image-manager/img/gallery-1.jpg
* Optimized: https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=conditional-orientation

* Pristine in landscape: https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg
* Optimized: https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg?impolicy=conditional-orientation
 
### Resize to 500 pixels while maintaining aspect ratio, and compress image with perceptual quality 
* Pristine: https://developer.akamai.com/image-manager/img/gallery-1.jpg
* Optimized: https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=resize&width=500

### Dynamic crop 
* Pristine: https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg
* Optimized: https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg?impolicy=crop&width=160&height=180&y=70&x=700


## Sample JSON IM policy Files

### [dacimg_crop.json](json/dacimg_crop.json)

Allows cropping any images based on parameters passed by query-strings, and optimizes the resulting image applying "perceptualQuality"="mediumHigh" (which translates to great byte savings).

For example if my pristine image is [https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg](https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg), I could crop the section that shows the second speaker by adding the following query string to the URL: `?impolicy=crop&width=160&height=180&y=70&x=700`. Here is the transformed image: [https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg?impolicy=crop&width=160&height=180&y=70&x=700](https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg?impolicy=crop&width=160&height=180&y=70&x=700)

Here is the explanation of the query string parameters:
* `impolicy`: built-in parameter used to manually indicate which policy should be applied (`crop` on this case)
* `width`: variable parameter used to indicate the desired width of the output image
* `height`: variable parameter used to indicate the desired height of the output image
* `y`: variable parameter used to indicate the desired vertical coordinate where the cropping should start
* `x`: variable parameter used to indicate the desired horizontal coordinate where the cropping should start

### [dacimg_filter.json](json/dacimg_filter.json)

Detects and crops the biggest face in the picture, converts the image to grayscale (black and white), and optimizes the resulting image applying "perceptualQuality"="mediumHigh" (which translates to great byte savings).

For example if my pristine image is [https://developer.akamai.com/image-manager/img/gallery-1.jpg](https://developer.akamai.com/image-manager/img/gallery-1.jpg) (a high quality stock image which is 7.14 MB), the resulting image of applying the `filter` policy: [https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=filter](https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=filter) would result on a black and white zoom of the person's face which is just 85.42 KB (almost 99% less bytes than the original), that is an amazing breakthrough your users are going to love.

### [dacimg_resize.json](json/dacimg_resize.json)

Allows resizing any images based on parameters passed by query-strings, and optimizes the resulting image applying "perceptualQuality"="mediumHigh" (which translates to great byte savings).

For example if my pristine image is [https://developer.akamai.com/image-manager/img/gallery-1.jpg](https://developer.akamai.com/image-manager/img/gallery-1.jpg) (a high quality stock image which is 7.14 MB), the resulting image of applying the `resize` policy with a parameter of width=500 (`?impolicy=resize&width=500`) would be a transformation of the original image which is 500 pixels wide, and just 51.13 KB (over 99% less bytes). Here is the transformed image: [https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=resize&width=500](https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=resize&width=500)


### [dacimg_watermark.json](json/dacimg_watermark.json)

Allows automatically creating a composite/watermark by combining a pristine image and the watermark image (usually the watermark is a smaller image with a transparent background which you want to add to high value images to protect the intelectual property value of the image). We would also apply "perceptualQuality"="mediumHigh" (which translates to great byte savings).

For example if my pristine image is [https://developer.akamai.com/image-manager/img/gallery-1.jpg](https://developer.akamai.com/image-manager/img/gallery-1.jpg) (a high quality stock image which is 7.14 MB), and my watermark image is the Akamai logo [https://developer.akamai.com/sites/default/files/2018-08/WebsiteImage-2_WT18_1.png](https://developer.akamai.com/sites/default/files/2018-08/WebsiteImage-2_WT18_1.png), the resulting image of applying the `watermark` policy [https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=watermark](https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=watermark) woukd show an scaled akamai logo at 20% of the size of the pristine image that will show  on the top left corner of the pristine image.


### [dacimg_conditional-letterbox.json](json/dacimg_conditional-letterbox.json)

This policy showcases a conditional transformation. It automatically detects if the image is in portrait mode, resizes the image to 300x300 keeping the aspect ration (note that is a square format) and automatically fits and fills the empty space. We would also apply "perceptualQuality"="mediumHigh" (which translates to great byte savings).

For example if my pristine image is [https://developer.akamai.com/image-manager/img/gallery-1.jpg](https://developer.akamai.com/image-manager/img/gallery-1.jpg) (a high quality stock image which is 7.14 MB), by applying the query-string `?impolicy=conditional-letterbox`: [https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=conditional-letterbox](https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=conditional-letterbox) (click and see the resulting image)

### [dacimg_conditional-orientation.json](json/dacimg_conditional-orientation.json)

This policy showcases a conditional transformation. It automatically detects and rotates 90 degrees images which are in landscape mode.

For example if my pristine image is [https://developer.akamai.com/image-manager/img/gallery-1.jpg](https://developer.akamai.com/image-manager/img/gallery-1.jpg) (a high quality stock image in portrait mode), by applying the query-string `?impolicy=conditional-orientation`: [https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=conditional-orientation](https://developer.akamai.com/image-manager/img/gallery-1.jpg?impolicy=conditional-orientation) the resulting image won't be rotated because the pristine image is not in landscape mode. However if we try the same policy on an image that is in landscape mode like [https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg](https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg) we will see it rotated. Click [https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg?impolicy=conditional-orientation](https://developer.akamai.com/sites/default/files/styles/blog_featured_image/public/2018-08/Blog-Webinar-Image-manager-R1.jpg?impolicy=conditional-orientation) to see the resulting image 

## Piez

Piez is a Google Chrome extension that is very useful for debugging and estimating the value provided by Image Manager and a few other Akamai features. You can find Piez in the [Google Chrome Store](http://bit.ly/2LQbLpG)

## Image Manager sample API commands

In the examples below we use HTTPie (http), and it's edgegrid extension: a very useful CLI tool to explore APIs. Another tool mentioned below is jq, which is another CLI tool that will help us filter and format JSON. See [this Blog](http://bit.ly/2Odo1lA) for instructions on how to install and use HTTPie and jq

 
### List all Image Manager policies configured on the Policy Set "developer_akamai_com-4903103" (replace that with the right name for your policy set)
```
http --auth-type=edgegrid -a dac-imaging: :/imaging/v2/policies Luna-Token:developer_akamai_com-4903103
```  
> See [OUTPUT](json/dacimg_ALL-policies.json)


### Let's add the "session" parameterm so we can simplify commands going forward
```
http --session=dacimg --auth-type=edgegrid -a dac-imaging: :/imaging/v2/policies Luna-Token:developer_akamai_com-4903103
```  
> See [OUTPUT](json/dacimg_ALL-policies.json)


### Voila! We can now reference the session "dacimg" going forward and save some typing. Here is the same command as before: 
```
http --session=dacimg :/imaging/v2/policies 
```  
> See [OUTPUT](json/dacimg_ALL-policies.json)


### Save all the policies into a file (in our example the file is commanded `dacimg_ALL-policies.json`)
```
http --session=dacimg :/imaging/v2/policies > dacimg_ALL-policies.json
```


### Filter the policy names 
```
http --session=dacimg :/imaging/v2/policies | jq .items[].id 
```
> See [OUTPUT](json/dacimg_quoted-policy-names.txt)


### Filter the policy names and remove the double quotes 
```
http --session=dacimg :/imaging/v2/policies | jq .items[].id | sed 's/"//g'`
```
> See [OUTPUT](json/dacimg_policy-names.txt)


### Loop through all the policy names and save each of them on a separate json file
This is the command I ran to generate all the json files I am including on this repository
```
for policy in `http --session=dacimg :/imaging/v2/policies | jq .items[].id | sed 's/"//g'`; do http --session=dacimg :/imaging/v2/policies/${policy} | jq . > dacimg_${policy}.json ; done
```

### Create a clone of the `conditional-orientation` policy called `conditional-orientation2` which rotates portrait images 90 degrees
```
sed 's/landscape/portrait/' dacimg_conditional-orientation.json > dacimg_conditional-orientation2.json
```
> See [OUTPUT](json/dacimg_conditional-orientation2.json)


### Submit a new policy called conditional-orientation2 out of the json generated above
```
http --session=dacimg PUT :/imaging/v2/policies/conditional-orientation2 < dacimg_conditional-orientation2.json
```
> See [OUTPUT](json/dacimg_conditional-orientation2_created.json)


### List all the policies and ensure the new policy is there
```
http --session=dacimg :/imaging/v2/policies | jq .items[].id
```
> See [OUTPUT](json/dacimg_quoted-policy-names2.txt)


### Delete the newly created policy called conditional-orientation2
```
http --session=dacimg DELETE :/imaging/v2/policies/conditional-orientation2 
```
> See [OUTPUT](json/dacimg_conditional-orientation2_deleted.json)

