# DNAC Configuration Templates Explained
<details><summary>Description</summary>
This repository will describe the process of updating Cisco IOS-XE Device Configurations using CLI, then demonstrate the same process using DNA Center Configuration Templates, and finally by using automation within our DNA Center Configuration Templates . First, we will look at updating configurations using CLI, the traditional method for updating Cisco Network Devices. Second, we will demonstrate how our knowledge of Cisco CLI, can help us create DNA Center Configuration Templates, so that we can update devices more efficiently using DNA Center. In the second section, we will quickly dip into automating portions of our templates using the templating languages, Velocity and Jinja, that are available for use in DNA Center's Template Editor.
</details>

## In the beginning there was CLI, and in the end... there will be CLI.
Stephen King once wrote, "...good things never die", and after observing the slow evolution of network device configurations over the last 20 years, I would have to agree. Despite fancy new GUI's that can be used to manage devices, network engineers are still choosing to use CLI to make their configurations. In recent years, Cisco released their latest network management tool, DNA Center, to make it easier for network engineers to monitor, troubleshoot, and manage their network. I have created this repository to demonstrate how Cisco is trying to bridge the gap between DNA Center's GUI and traditional CLI with the introduction of DNA Center Configuration Templates. These templates allow engineers to leverage their knowledge of CLI and build templates that can help automate the process of network device configurations.

  ### Traditional Configuration

  **Mission Statement:** 
  > Create and configure VLAN 20, and statically assign VLAN 20 to interface GigabitEthernet2/0/1 on a switch.
<details><summary>Traditional Configuration...</summary>
  In this example, we will start by looking at how we would traditionally configure VLAN access on a port, using a Cisco Catalyst 9300 Switch.

  **First, we will create and configure our VLAN that we want to add to our switch port:**
  ```
  Device# configure terminal
  Device(config)# vlan 20
  Device(config-vlan)# name test20
  Device(config-vlan)# media ethernet
  Device(config-vlan)# end

  ```

  **Lastly, we will statically assign our newly created VLAN to an access-port on our switch:**
  ```
  Device# configure terminal
  Device(config)# interface gigabitethernet2/0/1
  Device(config-if)# switchport mode access
  Device(config-if)# switchport access vlan 20
  Device(config-if)# end
  ```


</details>

  ---

  ### Traditional Configuration - Conclusion
  
  
<details><summary>Conclusion...</summary>
  Using CLI, we were easily able create and configure vlan 20, our new vlan. After we created our new vlan, we were then able to statically configure one of our switch ports, and allow access to the newly created vlan 20. 

  **For an efficent network engineer, this configuration would take less than a couple minutes. That was _easy!_**

  ![That was Easy](https://media1.giphy.com/media/zcCGBRQshGdt6/giphy.gif?cid=ecf05e47pzyh31ubbtwr7iqqpzff51whl96cihmbwo8dtmkf&rid=giphy.gif&ct=g)


  However, most of the time, we have to configure more than one switch...

  **Lets say we need to configure 300 switches** with this same configuration.

  ![Sweating GIF](https://c.tenor.com/Fp0JJdxY6msAAAAC/yes-sweating.gif)


  With some quick math, we can estimate, in a perfect world, it will take at most 600 minutes, or 10 hours, to complete this simple task across 300 switches! As we can see, even the most simple configurations can take hours, or even days to deploy across an entire network.


  But, what if we were able to create a template that allowed us to choose the VLAN we wanted to create, and the port that we wish to configure? And what if we could apply that template to multiple devices in our network at the same time? How quickly could we configure our 300 switches? Let's dive into DNA Center Configuration Templates and find out!

</details>

  ---
  
## Templates - The Bridge Between CLI and Mass Configurations.


### The Most Basic Template

**Mission Statement:**
> **Use a template to create and configure VLAN 20, and statically assign VLAN 20 to interface GigabitEthernet2/0/1 on 300 switches.** 


<details><summary>The most basic template...</summary>
  
I've briefly mentioned that a network engineer can leverage their knowledge of CLI to construct templates. So when we speak about constructing the most basic template - it is just CLI. When would you use just CLI? Let's start with our previous example:

> **Create and configure VLAN 20, and statically assign VLAN 20 to interface GigabitEthernet2/0/1 on 300 switches.** 

In this case, if we need to configure 300 switches with the exact same configuration, the challenge is not the actual configuration, but the _distribution_ of the configuration to 300 different devices. This is where a basic CLI template can assist us. We can simply go into the DNA Center Template Editor, create a new template, and write the **_configuration commands_** to the template.

Below is an example of what that would look like:
![IMAGE OF BASIC CLI TEMPLATE](/images/basic_cli_template.png)

As you can see, the template does not look exactly like the CLI we entered in the fist example, in fact, we only needed to copy the commands that are entered after 'Configure Terminal', and we did not have to use the 'end' statement between the configuration of the VLAN and Interface.

Now that we have created our configuration, we can save and commit it. Once the template has been committed, we are able to apply it to a network profile, and deploy the template to all of the devices that meet the requirements of our template. Using a basic CLI template, we are able to solve our distribution challenge that we would have encountered without the use of templates.


</details>

  ---

### The Most Basic Template - Conclusion


<details><summary>Conclusion...</summary>
---

**The most basic form of a template is just CLI. To understand if you need to use a basic CLI template, ask yourself these questions:**

1. Does _any_ part of the configuration need to change between devices?
2. Do I need to apply the same configuration across multiple devices?


If you answered **_NO_** to the fist question, then you can probably use a basic CLI template to push your configuration to device(s) in your network.

If you answered **_YES_** to the second question, then it is probably best to use a template, so that your configuration can be easily disributed across multiple network devices.

---

Asking these questions will allow us to determine the best method to configure our network devices. That being said, what is the best configuration method if our answer to the fist question was **_YES_**? Let's talk about template languages, and template variables.


</details>

  ---
### Template Language Options

<details><summary>Template Language Options...</summary>

Before we can dive into variables, we first need to learn about the template languages that are availble in DNA Center Configuration Templates.

When you first create a template in DNA Center, you will have to choose which template language you would like to use. The two choices are:
* Velocity
* Jinja

Velocity and Jinja are templating languages that allow for the creation of dynamic templates. Without diving into the weeds, I can say that the biggest difference betweeen the two languages is the syntax of the languages. 

Some people may ask, **"_which language is better?_"**, and the answer to that question depends on the user. If you have used either Velocity or Jinja in the past, I would say it's probably best to choose the language that you are most comfortable using. However, if Velocity and Jinja are both foreign to you, I would suggest picking up Jinja. Jinja is the newest templating language added to DNA Center, and it allows for more complex logic than Velocity.
For that reason, I will be using Jinja as the template language of choice for all examples beyond this point. 

If you would like to learn more about Velocity, you can read the [Velocity User Guide](https://velocity.apache.org/engine/1.7/user-guide.html).

If you would like to learn more about Jinja, you can read the [Jinja User Guide](https://jinja.palletsprojects.com/en/3.0.x/templates/).

Now that we have chosen the template language to use, lets dive into Jinja _variables_!


</details>

  ---

### Variables

**Mission Statement:**
> Create and configure VLAN 20, and statically assign VLAN 20 to any interface on a switch.


<details><summary>Variables...</summary>
  
Let's start by recalling our initial mission:
> Create and configure VLAN 20, and statically assign VLAN 20 to interface GigabitEthernet2/0/1 on a switch.
  
Our objective was to configure our switch by creating a specific VLAN, VLAN 20, and then assign that VLAN to a specific interface, interface GigabitEthernet2/0/1. 

Our next mission was to apply the same configuration across 300 different switches. However, in the real world, it is not very realistic to assume that we will be applying VLAN 20 to the same exact interface across 300 switches. Some switches may need to have Interface GigabitEthernet2/0/1 updated, and some may need to have GigabitEthernet1/20 updated. 

If this is the case, we need to ask ourselves, "what is most efficient way to create and apply VLAN 20 across **_ANY_** possible interface?"

**The answer to that question is...**

![Variables - Spongebob](https://i.imgflip.com/6i0bxw.jpg)

Let's take a look at our next mission, and how we can accomplish it using DNA Center Configuration Templates with Jinja.

Mission Statement:
> Create and configure VLAN 20, and statically assign VLAN 20 to any interface on a switch.

Below is an example of what the Jinja template would look like:
![IMAGE OF BASIC Jinja Variable TEMPLATE](/images/basic_variable_template.png)

Let's dissect the template so that we can understand what is happening.

First, we have our traditional CLI, which is creating the VLAN:
```
vlan 20
name test20
media ethernet
```

Next, we have our traditional CLI which is incorporated with our Jinja Variable, "interface_variable". 
```
interface {{interface_variable}}
```

As you can see, our Jinja Variable is surrounded by **two curly brackets** **_{{ }}_** on each side. The curly brackets denote where Jinja is supposed to take over. Therefore, when the template is deployed to a device, it will first ask you to assign **interface_variable** a value. 

Finally, we have the rest of our configuration lines are traditional CLI:

```
switchport mode access
switchport access vlan 20
```
</details>

  ---

### Variables - Conclusion
<details><summary>Conclusion...</summary>
  
  
**Let's run our new Jinja template, and see how it works:**
![IMAGE OF BASIC Jinja Variable TEMPLATE](/images/basic_variable_form.png)

As we can see, when we go to run our template, we are asked to first give **interface_variable** a value. I have given it the value of **_GigabitEthernet1/20_**.


**Now let's see the configuration that is applied to our device:**
![IMAGE OF BASIC Jinja Variable TEMPLATE](/images/basic_variable_run.png)

As you can see, the variable in our template was replaced by the given value "**_GigabitEthernet1/20_**", and our configuration was applied successfully.

---

**To understand if you could replace a portion of a CLI command with a variable, ask yourself these questions:**
1. Would a portion of the template need to be edited if I applied this configuration to another device?
2. Would a portion of the template need to be edited if I applied this configuration in the future?

If the answer is **_YES_** to either question, then you should consider using variables for the portions of your template that are subject to change.

**To create future-proof templates, ask yourself:**
1. Which parts of this template could be replaced with variables so that it can solve for a more generic usecase in the future?

---

Using the power of variables, we were able to create a dynamic configuration, that is able to apply our VLAN 20 to **_ANY_** interface that we choose! Remember, this is just a simple example of how variables within DNA Center Templates can help simplify configuration deployments to devices. Also keep in mind, we are not limited to the number of variables inside of a template. If we had to accomplish a similar mission in the future, which portion of the template could we replace with variables to make it more generic? 

**_Hint: VLAN Number._**



### Congratulations, you have completed the first section of this repository! If you want to dive deeper into DNA Center templates, please follow the link below. 

</details>

  ---
  
  
[Continue to the next section!](/sections/more_about_templates.md)
  

