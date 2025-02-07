+++
date = '2024-10-26T16:00:00+01:00'
draft = false
title = 'First steps with a fresh Ubuntu 24.04'
+++

I just installed Ubuntu 24.04 LTS on a 2015 MacBook Air to give it a second life. Surprise! All the laptop's functionalities work perfectly: trackpad, WiFi, special function keys, and even sleep mode when closing the lid. Additionally, the system's speed is noticeably superior compared to macOS.

Now that I have a fresh Ubuntu installation, what steps should I follow? After researching online, I've gathered some of the recommended first tasks:

Updating the system
Keeping the system updated is very important to get the latest security patches and an optimized system.

**1. First, update the local package index by running the following command in the terminal:**

```sudo apt update```

Then, upgrade the packages to their latest versions with this command:

```sudo apt upgrade```

Finally, open the App Center to check if any applications have pending updates.

**2. Get familiar with the GNOME environment**
Although I have used Linux and specifically Ubuntu before, I use Windows daily. GNOME is very intuitive, but it's a good idea to spend some time exploring its options, menus, applications, and generally getting familiar with the system.

**3. Customize the desktop**
In the Settings menu, you'll find tools to personalize the system. The Appearance section is especially useful, where you can enable dark mode, change the wallpaper, and adjust the position of the taskbar.

**4. Install your preferred software**
The easiest way to install software is by using the App Center, where you'll find many popular applications.

In my case, I have already installed some basic applications that I use frequently and will add more as needed:

- **Brave**, this browser looks great, and I want to try it. Ubuntu comes with Firefox by default.
- **Visual Studio Code**, a very versatile software development environment.
- **Element**, a Matrix client.

**5. Security first: Enable and configure the firewall**
Ubuntu includes the UFW (Uncomplicated Firewall), which is very simple to configure and effective. To configure it, use these terminal commands.

To check the current firewall status:

```sudo ufw status```

By default, it will be inactive, so activate it with this command:

```sudo ufw enable```

Then, set a minimal policy by denying incoming connections by default and allowing outgoing ones:

```sudo ufw default deny incoming```

```sudo ufw default allow outgoing```

Next, allow exceptions for browsing the Internet (enabling HTTP and HTTPS ports) and for accessing the computer via SSH:

```sudo ufw allow http```

```sudo ufw allow https```

```sudo ufw allow ssh```

**6. More advanced steps**
Many guides mention the possibility of enabling **Flatpak**, an alternative to Snap, Ubuntu's default package manager. Each has its advantages and disadvantages, so it's advisable to do some research before making a decision. [You can find more details about both package systems at this link](https://itsfoss.com/flatpak-vs-snap/).

**Conclusion**
My last experience with Ubuntu was a few years ago, and now that I’ve returned to it, I can say the evolution has been impressive. It has improved in compatibility with older hardware and now offers a much more polished and efficient experience. In my case, installing Ubuntu on my MacBook Air was a great decision: the device went from being barely usable to having excellent speed, ease of use, and full compatibility.

With these steps, you now have a solid foundation to start exploring Ubuntu!