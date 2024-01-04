# How to run nginx? Host vs Container

Consider running nginx within a docker container. Depending on the
infrastructure you plan to setup, the traffic you anticipate on your site (if
high). And the available resources on your server. And if a single server is
sufficient or multiple hosts are needed.

Advantages:
* Scalability: Scaling horizontally is easy. If you need to handle more
  traffic, you can simply start more Nginx containers.
* Reproducibility: Containers are lightweight and portable. So you configure
  the image and it runs the same no matter the host.
* Isolation: Isolated environment, helps prevent conflicts between different
  services and adds security.
* Version Control: Updating is decoupled from the hosts package manager and can
  be done by simply rebuilding the image and deploying a new container. Going
  back to an older version, if for whatever reason is needed, is just as simple.

Disadvantages:
* Complexity: While Docker simplifies many aspects of deployment, it can also
  add complexity, especially when managing inter-container communication and
  shared resources 

> 🧐 If you choose to use docker just don't add multiple sites into a single
> container

Related:

* [20240104194645](/20240104194645/) Grok nginx
* [20240104154437](/20240104154437/) Server Service: Add nginx
* [20240104162118](/20240104162118/) Add https cert for nginx
* [20240104161046](/20240104161046/) Fresh Server: First Steps

Tags:

      #linux #server #webserver #nginx #container
