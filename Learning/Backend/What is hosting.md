#### What is the difference between web page, website, web server, and search engine?
- **Web page**: a document which can be displayed in a web browser. Often called just "page".
- **Website**: a collection of web pages which are grouped together and usually connected together in various ways, such as sharing a unique domain name. Often called a "website" or a "site".
- **Web server**: a computer that hosts a website on the internet.
- **Search engine**: A web service that helps you find other web pages.

Source: *[MDN](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/Pages_sites_servers_and_search_engines)*

---
#### What is web server
The term *web server* can refer to hardware or software, or both of them working together.

1. On the hardware side, a web server is a computer that stores web server software and a website's component files (e.g. HTML documents, images, CSS stylesheets, and JavaScript files). A web server connects to the internet and supports physical data interchange with other devices connected to the web.
2. On the software side, a web server includes several parts that control how web users access hosted files. At a minimum, this is an *HTTP* server. It understands *URLs* and *HTTP*. An HTTP server can be accessed through the domain names of the websites it stores, and it delivers the content of these hosted websites to the end user's device.

At the most basic level, whenever a browser needs a file that is hosted on a web server, the browser requests the file via HTTP. When request reaches the correct (hardware) web server, the (software) HTTP server accepts the request, finds requested document and sends it back, also through HTTP.
#### Static vs dynamic web server
To publish a website, you need either a static or a dynamic web server.

A **static web server**, or stack, consists of a computer (hardware) with an HTTP server (software). We call it "static" because the server sends its hosted files as-is to your browser.

A **dynamic web server** consists of a static web server plus extra software, most commonly an application server and a database. We call it "dynamic" because the application server updates the hosted files before sending content to your browser via HTTP server.
#### Hosting files
First, a web server has to store the website's files. Technically, you could host all those on your own computer, but it's far more convenient to store files all on a dedicated web server because:
- A dedicated webserver is typically more available (up and running).
- Excluding downtime and system troubles, a dedicated web server is always connected to the internet.
- A dedicated web server can have the same IP address all the time. This is known as a *dedicated IP address*. (Not all ISPs provide a fixed IP address for home lines.)
- A dedicated web server is typically maintained by a third party.

#### Communicating through HTTP
Second, a web server provides support for HTTP. As it name implies, HTTP specifies how to transfer hypertext (linked web documents) between two computers.
On a web server, the HTTP server is responsible for processing and answering incoming requests.
1. Upon receiving a request, an HTTP server checks if the requested URL matches an existing file.
2. If so, the web server sends the file content back to the browser. If not, the server will check if it should generate a file dynamically for the request.
3. If neither of these options are possible, the web server returns an error message to the browser, most commonly `404 not found`.

Source: *[MDN](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_web_server)*

---
#### Where to Host a Fullstack Project on a Budget
#### Summary
#### Highlights
- ☁️ **Cloud Providers**: Leverage free tiers from AWS, Google Cloud, and Azure for initial project hosting.
- 💰 **Budget-Friendly**: Self-hosting can save money; focus on setting up your own services.
- 🛠️ **Flexibility**: Use VPS for databases and servers, allowing customization and scalability.
- 📊 **Performance**: Benchmark different VPS providers to find the best performance-to-cost ratio.
- 🌍 **Static Hosting**: Use Netlify for static front-end sites to reduce server load and costs.
- 🖼️ **Image Handling**: Consider services like Cloudinary and ImageX for dynamic image resizing and storage.
- 📈 **Long-Term Growth**: Plan for future scalability while starting on a budget with cloud services.
#### Key Insights
- ☁️ **Cloud Free Tiers**: Utilizing free tiers from major cloud providers can significantly lower initial costs, making them ideal for low-traffic projects. This strategy allows developers to test and grow their applications without immediate financial strain.
- 💰 **Self-Hosting Benefits**: Setting up your own server and database on a VPS can provide long-term savings. Although it requires more technical knowledge, it offers flexibility and control over resources and configurations.
- 🛠️ **Complexity of Cloud Services**: While cloud services provide extensive features, their complexity can lead to unexpected costs. Understanding their pricing models and potential pitfalls is crucial for maintaining a budget.
- 📊 **Benchmarking VPS Providers**: Performance can vary widely among VPS providers, so conducting benchmarks is essential. This helps ensure you choose a service that meets your performance needs without overspending.
- 🌍 **Static vs. Dynamic Hosting**: For static sites, using dedicated platforms like Netlify is cost-effective and efficient. For dynamic sites, integrating your front-end with a VPS can streamline operations and reduce costs.
- 🖼️ **Image Storage Solutions**: Choosing the right image hosting service affects both performance and cost. Services like Cloudinary offer robust features but may complicate pricing, while ImageX provides straightforward charges based on usage.
- 📈 **Scalability Considerations**: Start with budget-friendly solutions, but consider long-term growth. As your project gains traction, transitioning to paid services may be necessary, so plan accordingly to avoid service interruptions.

Source: *[Ben Awad, YouTube](https://www.youtube.com/watch?v=Kx_1NYYJS7Q)*