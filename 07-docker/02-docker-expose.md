## What is the purpose of EXPOSE in Dockerfile?

### Question  
What is the purpose of the `EXPOSE` instruction in a Dockerfile?

### Short explanation of the question  
This question is meant to test your understanding of how container networking works and how ports are communicated within and outside the container runtime.

### Answer  
`EXPOSE` is a Dockerfile instruction used to indicate which port the containerized application will listen on at runtime. It serves as **documentation** and a **signal** to tools like Docker and Docker Compose, but it does **not** actually publish the port.

### Detailed explanation of the answer for readers’ understanding

The `EXPOSE` directive does **not open the port** to the outside world by itself. Instead, it tells Docker that the container is expected to listen on the specified port(s).

To make a port accessible to the host or external clients, you still need to use the `-p` or `--publish` flag when running the container:

```bash
docker run -p 8080:80 myapp
```

In this case:
- `80` → is the port inside the container (maybe defined using `EXPOSE`)
- `8080` → is the port exposed on the host machine

---

### 🔧 Example: Dockerfile with EXPOSE

```Dockerfile
FROM nginx:alpine
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

The container is configured to listen on port `80`, but if you run it as:

```bash
docker run mynginx

*************************
Like if you remember, docker file has keywords like run cmd entrypoint.

Similarly, there is a keyword for expose.

Let's say you are working on a web application that you want to deploy on nginx, and if the web application

is running on port 80.

So you use expose 80.

But what exactly is expose 80?

Because end of the day, when you want to run the container and you want to access the application from

your host, you use something like docker run hyphen P 80, colon 80.

So you are mapping the port in the container with the port on your host.

So when you are using hyphen p.

What is the purpose of expose?

Does it actually make any sense within your Docker file?

So when interviewer asked this question, a lot of people get this wrong because the practical use case

of expose is not understood by many.

Let me tell you how you can answer this question to the interviewer, and then I will explain you further.

In docker file, expose instruction or expose keyword is just used for documentation purpose.

So let's say you are working on a docker file, and year later you want to hand over this docker file

to other DevOps engineer.

So expose keyword will help the other DevOps engineer understand that your application is running on

port 80, because your application can run on any port.

Other DevOps engineer can't go to the source code.

understand on which port is the application running.

Instead, Dockerfile allows documentation of the port using expose instruction.

So this is what you tell the interviewer.

It is more of metadata where it will help the other people who are working on the Dockerfile understand

which port is the application running on.

It does not actually publish or open port to the outside world.

Many people think when you use the export keyword and you when you mention the port, it exposes the

port to outside world.

But no, it does not do it.

That is taken care by hyphen, p keyword or parameter, not keyword.

So when you actually run the container, you use docker run followed by hyphen p.

And this is taking care of publishing the port.

Mapping the port of the port in the container with the port on the host.

The interviewer might ask you, okay, when it comes to Docker, there might not be great benefit except

for documenting, but is there any other use case of the expose keyword?

Now things here turn a little complicated because interviewer is diving deep into the topic.

In this case you can talk about use of expose keyword when you are working with Docker compose.

So to be honest, expose keyword comes into handy only with docker compose, because with docker file

it is mostly doing metadata related thing.

But when you're dealing with Docker compose, let's say you are working with a front end and back end

in the docker compose file.

If you use expose keyword in the back end docker file, and if you say expose 8080, then docker compose

will automatically understand that front end needs to talk to back end on port 8080.

Or maybe you have database and database in the docker file you expose 3306.

Then docker compose understands that back end needs to interact with db on port 3306.

As I told you, it is for metadata purpose.

So docker compose reads this metadata and understands things like this in a nutshell.

Like if you are confused, let me just repeat it.

If you are talking in terms of docker file because the interviewer question is about docker file, expose

keyword does not have a lot of benefit.

It is just an instruction kind of metadata that will help other people who are reading the docker file

understand on which port is the application running.

A lot of people confuse that it is used for publishing the port.

No for publishing and mapping the port onto the local machine, you use hyphen P while running the docker

run command.

However, in future when you deal with docker compose where one service is talking to another service

such as backend is talking to db.

If you have expose keyword or sorry, expose a keyword in the docker file or instruction in the docker

file, then one service understands that other service is listening on that particular port.

Front end might understand backend is accessible on this port.

This happens because docker compose reads the expose instruction from the docker file.

This is how you answer the question.

I hope this is clear.

As I told you, 90% of people get it wrong because of the name.

When they see the name expose, they assume it is used to expose the port.

And this is where people go wrong.


```

It won’t be accessible externally unless you publish the port:

```bash
docker run -p 8080:80 mynginx
```

---

### 🧠 Real-world Insight

> “We used EXPOSE in all our base images so that developers and Docker Compose could understand the default ports our microservices used — even though we always mapped them explicitly with `ports:` in Compose.”

---

### Key takeaway

> "`EXPOSE` is a form of **documentation** inside the image to signal expected ports — it doesn't open ports on its own."
