![image](https://user-images.githubusercontent.com/40838156/226753837-14b460c1-20f6-4fc1-a449-c61dd55a2177.png)

Has someone told you that “You should really start doing side projects to build your portfolio” and deep down you really want to but are not sure how to go about it? I think we all want to build something useful and when we start learning about software development we just have this desire to build something. However, as we sit there rattling our brains we cannot seem to come up with an idea and end up stuck in [tutorial hell](https://www.google.com/search?q=tutorial+hell&oq=tutorial+hell&aqs=chrome..69i57.2071j0j1&sourceid=chrome&ie=UTF-8).

My friend and I got an email about a non-profit organization called [Raison d’art](https://www.raisondart.org/about) who was looking for volunteers to help develop an internal tool to help them manage their image assets in their S3 bucket. A key thing that drew us to this project was that the organization wanted to make the project open source and the thought of starting an open-source project seemed really exciting to us! We had never used S3 before but we both had this desire to build something and thought this was a great opportunity to test our problem-solving skills while also helping others. So we decided to go ahead and start the project. 

![Let's do this gif](https://3qz76o35jsxd224d852obgtp-wpengine.netdna-ssl.com/wp-content/uploads/2020/03/giphy-3.gif)

If you do not know about [Amazon Web Services (AWS)](https://aws.amazon.com/) or what S3 is here is a quick summary. AWS is a platform that supports **many** secure cloud services for just about anyone to use. As you probably guessed, S3 is one of these services. It is primarily used as storage on the cloud for just about anything. You can read more about it on [AWS’s S3 documentation](https://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html). 

Since this project was during the start of COVID-19 pandemic, everyone in the team only met virtually. During the first meet up in June 2020, we discussed what exactly we wanted to develop and how it would solve the problem Raison d’art was having. We decided to build a React application since both my friend and I knew Javascript and were both somewhat familiar with React. 

I remember being unsure since we did not know if the design decisions we would make were the best ones. There is a lot of **imposter syndrome** when you try developing any bigger feature in a project and this was no exception. 
> I think the best way to overcome imposter syndrome is to just try something out and then share your ideas with others to get feedback.

This way you have something to show and learn from while building momentum in the project. We would meet every two weeks to discuss the key features we wanted to tackle in the coming weeks and where we were at in the project. It was great to see how the project kept growing every time we met and to hear others' feedback. I think something I would suggest to others is to write down the problem you are trying to solve and break it down into smaller high-level parts. This way you have something to refer back to you while you are developing.

![Just do it gif](https://media3.giphy.com/media/UqZ4imFIoljlr5O2sM/giphy.gif)

Something we started learning quickly about AWS was just how much there was to read. The sheer amount of documentation AWS has about their products can be overwhelming. **But you should 100% read this documentation**. This is how we discovered that [Tags](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-object-tags.html) were better suited for the organization’s assets instead of using [Metadata](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html) (which was the original plan). Since an object’s metadata is tied to the object in the bucket it means if you wish to edit this piece of information you need to re-upload the object every time you edit just a single piece of metadata. In layman's terms, there was a lot of overhead to edit just a single piece of data (since the metadata cannot be edited after the initial upload so you have to replace the existing object). However, tags are not stored with the object so if you wish to just modify a single tag of an object you can simply edit that tag without having to reupload the whole object. Check out the documentation about [metadata](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html) versus [tags](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-tagging.html) for reference. 

Another feature we wanted to develop was to make it possible for several objects to have similar tags and defaults without having to manually add each one. S3 has no sort of inforced structure when it comes to tags and is very similar to NoSQL databases (more specifically a Key-value database) meaning as long as a unique key is provided the value can be anything. So this where the idea of the schema file came from. We essentially uploaded in a folder a JSON file we called `bucket-buddy-schema.json` (an example of the contents can be seen below) which would store what tags should be applied to each object in a given folder. 

```
[
   {
      "key":"Name",
      "Value":"default name",
      "type":"text"
   },
   {
      "key":"Age",
      "value":"23",
      "type":"number"
   },
   {
      "key":"DateOfBirth",
      "value":"2020-09-30",
      "type":"date"
   },
   {
      "key":"Validated",
      "value":false,
      "type":"flag"
   }
]

```
This file specifies the key (the tag name), value (the default value for the tag), and the type (which could be Numeric, Text, Date, or Flag) that each object should contain, and when viewing an object we clearly identify that these are the values needed to be added to follow the schema in a given folder.  

As the project was coming to an end and we began polishing the app. A key thing we wanted to do from the start was to build an app for developers and non-developers could use and this meant trying to build a README.md that was as easy to understand. We even added gifs showing all the features and a brief explanation on how to get started. Something I think is often overlooked in documentation is examples. As the saying goes, “A picture is worth a thousand words.” 

We even got the app set up so it can be wrapped as an [Electron app](https://www.electronjs.org/) so our React application could behave like an actual desktop app! Once everything was ready we were invited to a local meet-up called [JS-Montreal](https://js-montreal.org/about.html) to present our project and get feedback. It was a lot of fun to present our project to other developers in the community and get their feedback and it really showed how supporting the development community can be. 

![Lets wrap this up gif](https://media1.tenor.com/images/fc51042e9fb2a5aff907115b7175617b/tenor.gif?itemid=11112770)

If I learned anything from starting an open-source project from scratch is this, you learn more than just how to code. I discovered how to be creative while helping others and it felt so good to be able to solve a problem when I had trouble coming up with an idea for my own project. Knowing your weaknesses can be one of your greatest strengths! There are so many problems non-profit organizations and people are facing on a daily basis so if you are unsure on what project you should do and work on early in your career try to ask and listen to those around you. 

> At the end of the day being a developer means being a problem solver and being able to interpret someone’s problem so you can then design and present a solution.

Not only will you feel like you are building something useful that will help others reach their goals but you will be able to build that portfolio that everyone keeps suggesting you should have. Making a product open source makes it so you are not just helping the original person having the problem, but others too and gives the project the chance to turn into something you never thought it could be (and that thought is truly exciting). 

Thank you for reading this blog post and if you want to check out the Bucket Buddy repository I talked about in this post please check out the [GitHub repository here](https://github.com/Davescat/bucketbuddy)! Happy coding! 
