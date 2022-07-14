### For Loops

  **Mission Statement:**

> Create and configure VLAN 20, and statically assign VLAN 20 to **_ANY_ number of interfaces** on a switch.

<details><summary>For Loops...</summary>
  
Now that we have a basic understanding of variables, let' dive a little deeper into the power of templating. Let's take a look at **_For Loops_**.
**The purpose of a For Loop is to iterate through items in a list.** As we iterate through items in a For Loop, we can then execute commands on those items.

In our previous mission, our objective was to Apply VLAN 20 to any **_ONE_** interface on a device. We were able to accomplish this task using Jinja variables. Well What if instead we wanted to apply VLAN 20 to **ANY number of interfaces** on a device? Let's take a look at how we can accomplish this.

**Mission Statement:**

> Create and configure VLAN 20, and statically assign VLAN 20 to **_ANY_ number of interfaces** on a switch.

Below is an example of what the Jinja Template would look like:

![IMAGE OF JINJA FOR LOOP TEMPLATE](/images/jinja_for_loop_template.png)

Let's dissect the template so that we can understand what is happening.

First, we have our traditional CLI, which is creating the VLAN:
```
vlan 20
name test20
media ethernet
```

next, we have **the start** of a Jinja _For Loop_:
```
{% for interface_variable in interaces_list %}
```

everything that is after the above statement, and before the **end of the For Loop** will be run for every item in our list. In this case, each item in our list is an interface name, which is denoted by the jinja variable **interface_variable** that was declared in the above statement.

Therefore, our next lines of code should be the interface configurations:

```
interface {{interface_variable}}
switchport mode access
switchport access vlan 20
```

Finally, we will end our for loop:

```
{% endfor %}
```

  
</details>

  ---

### For Loops - Conclusion
<details><summary>Conclusion...</summary>
  

**Let's run our template:**
![IMAGE OF FOR LOOP VARIABLES FORM BEFORE LIST IS CREATED](/images/jinja_for_loop_form.png)

When we go to run our template, we are asked to first select the interfaces that we would like to apply our configuration to. However, there are **NO interfaces to select!?** Before we can select our interfaces from our list, we must first populate interface list using the DNA Center Template Form Editor. 

**Lets watch a quick video on how to access the form editor, and create our interface list:**
[![Watch the video](/images/for_loop_form.png)](https://youtu.be/hPEgaEnmQj8)




**Now let's watch a quick video - We will run the configuration and see what would be applied to our device:**
[![Watch the video](/images/for_loop_video.png)](https://youtu.be/Y6SOXtcq_KI)


As you just saw, we were able to accomplish the task of **_Create and configure VLAN 20, and statically assign VLAN 20 to any number of interfaces on a switch._** After running our template, we were able to apply the interface configurations to any number of interfaces. All we had to do was select the interaces that we wanted to configure! 

***

**To understand if you should use a _For Loop_ in your template, ask yourself these questions:**
1. Do I need to apply a configuration more than one time on a single device (I.E. the same configuration, but on different interfaces)?
2. Do I need to traverse a list of similar items on a device?

If the answer is **_YES_** to either question, then you should consider using _For Loops_ for the portions of your template that are subject to repeat configurations.


</details>



***

## Automation - A Brief Introduction to fully automated templates.

### System Variables

**Mission Statement:**
> Iterate through every loopback interface on any given switch, and update it's description


<details><summary>System Variables...</summary>
  
  
DNA Center is able to give us access to some of its device knowledge when we are constructing templates. It gives us access in the form of **_System Variables_**. To view a list of all system variables, click on the "System Variables" hyper link in the template editor screen, as shown below.
  
  
![IMAGE OF SYSTEM VARIABLE HYPER LINKS](/images/system_variables.gif)

For our Mission, we must iterate through all loop back interfaces on any given device. We will be using the **_\_\_interfaces** system variable. This System Variable is a **LIST** of every interface on the device. 
  
Now lets take a look at the template that can accomplish our mission:
![IMAGE OF SYSTEM VARIABLE TEMPLATE](/images/jinja_system_variable_template.png)

  
Let's dissect the template so that we can understand what is happening.
  
  First, we have our Jinja For Loop that is iterating through the __interfaces list:
  ```
  {% for interface in __interfaces %}
  ```
  
  Next, we have an IF Statement, that is checking if "Loopback" is within the port_name of the interface:
  ```
  {% if "Loopback" in interface.port_name%}
  ```
  
  If the word "Loopback" is contained in the interfaces name, it will change the description of that interface. If "Loopback" is not contained in the interfaces name, it will do nothing:
  ```
  description This is a Loopback interface!
  ```
  
  Finally, we close our if statement, and then our for statement:
  ```
  {% endif %}
  {% endfor %}
  ```
 
 
  
</details>

  ---
  
  
### System Variables - Conclusion

<details><summary> Conclusion... </summary>
  
  
  **Let's run our new Jinja template, and see how it works:**
![IMAGE No VAIRABLES NEEDED TO BE ADDED](/images/jinja_system_variable_run.png)

As we can see, when we go to run our template, we are not asked for any variables. All of our variables are already available via the DNA Center System Variables. The only thing we need to do is tell DNA Center which device we would like to pull the system variables from.
 

We were able to accomplish the task of **_Iterate through every loopback interface on any given switch, and update it's description._** After running our template, we were able to change the description of ONLY the Loopback interfaces, and we can apply that on ANY Switch that meets the device requirements of the template.
  
</details>
  
  
  ---
  
### Examples

This README walks through some very basic configurations, to introduce DNA Center Configuration Templates. If you want to see other ways templates can be used, please take some time to look through the available [examples](/examples).

---
