# Takeaways :
- why containers? packaging, distribution, isolation.
- Imperative vs. Declartive
- declaring a state
- Immutable infrustacture
- Infrsutature as a code.
- resource boundary vs secuirty boundary
</br></br></br></br>


# Kubernetes by Brendan Burns
- https://www.youtube.com/watch?v=q1PcAawa4Bg&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT


# What is Kubernetes?
- an orchestratir to help you with application development.
- deploying an applciation :
    - package it up
    - distribute it (on-pre, cloud, etc)
    - keep it running
    - load balnce the traffic between its pieces
    - use api to monitor it

- Kubernetes mainstreamed the idea of distributed systems application envirnorment to build distributed systems.
- what's container ?
    - taking an application and package it. so basically building a binary representation of different pieces you need for you app.(such as software, config files, etc).
    - and having a protocol for distributing that application around the wrold.
    - once it's running (on machine for example), it has isolated playground so it can't interfere with other apps.</br></br>

    - basiaclly container is 3 pillars :
        - how do I take everything and **`package`** it? so it's the same on my laptop or the could or whereever
        - how do I **`distribute`** it around the world easily?
        - once it's running, how do I have **`isolated`** env that if it has a bug it does't affect other apps ?

- what's cloud-native app ?
    - API-driven development.
    - you're creating things with API, you're not logging into machine & running commands
    - you're **`delcaring a state`** (usually text file, YAML), and then applying that state to the world and you're letting other systems take responsibility to make reailty to match the state you declared (this's called Infrustracture as code)
    - the idea of **`elstic scale`**. I can get resources immediatly, and I can throw them away immediatly.

- what's the future of Kubernetes?
    - it becomes default, the future is budling extentions/components on top of it.
    - in future, it'll fade as becoming norm. for example, X86 it's there, "it's there", not part of what u think of day-by-day



# Why you should care about containers
- In old days, VMs where mamanged and all its different components such as SSH server, OS, monitoring, you app, etc.
    - managing was through Imperative Script (ex: Bahs, Chef, Puppet).
    - it's something we run on VM.
    - it's differcult to understand if fails or why
- In that contex, new move began **`Immutable Infrustracture`**
    - ex : let's capture all different pieces in on VM image.
    - it was great from architcutre perspective, but heavy work from development point of view.
- Then, new move began.. let's take that vm image and turn it into **`Container Image`**
    - much lighter weight
    - separate the app from the kernel.
        - when I think about my app I think about the container.
        - when I need to think about OS I think about the kernel.
        - separation of concers made it easier for dev & artifacts
        - imutability
    - Cloud repo is anothe import benefit containers brought (distribution)
    - container image becomes a way of putting multiple apps on the same machine (utilization).
        - Rememeber :
            - it's resouce boundary that's defined in the kernel.
            - good way of isolation in terms of  things like memoery and cpu.
            - it's not a **`secuirty boundary`**, for example, it won't protect you from malicious code in other app !
                - if you need to use container as a secuirty boundary you need additional technologies like hypervisor or ther kind of isolation technology.


# How Kubernetes works
- Fundamentally, Kubernetes take bunch of vms/phsycial machines and transfer them into unifed API surface that developers can interact with, and with containers. without thinking about underlying machines.
- Kubernetes API presents different pieces, the core ones are :
    - **`Pod`** : collection of containers that co-located on a signle machine.
    - **`service`** : load balancer that can bring traffic down to collection of pods.
    - **`deployment`** : under the hood use a (replicaSets), which used for replicating the conatiners for availabitiy and scale. 
- use Kubectl (in general, you can use other cli tools) to interat with the AP. to make http call to Kubernetes API server.
- Example, let's assume we want to deploy web app, a container image. to run it on Kubernetes API:
    - create a "Deployment", usually represnted as YMAL file. and let's say 3 replicas
    - use kubectl to apply that deployment, that's gonna send it through API server.
    - 3 Pods will be created, which will result in "the scheduler" placing 3 container on the underlying vms/machines, host machines.
    - **`DeploymentObject`**, will manage and ensure those replicas in good state
    - To allow my app to be consumed, we need to create a service, which load balancer that knows who take traffic either from the outside world or from another service inside the cluster. We alos can configre the service if it's attached to an external load balancer,  Kubernetes will be responsible to talk to that load balancer provider, ex : cloud provider.</br></br>
    - what happens if we change deployment to have 4 pods ?
        - due the declaretive nature of Kubernetes, will see you have only 3, so it'll go and add new replica accordingly. 
        - our end user, won't notice we scaled up our app.</br></br>
    - we can do **`rolling deployment`** where we move fron v1 (of my app/container ) in  to v2 in place without the end user notcing any changes/intrupption.

# How Kubernetes deployments work
