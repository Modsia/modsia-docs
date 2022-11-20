Security Considerations
=======================

Most components run as the session user. The STT engine may be a daemon. Most components are discovered
and communicated with over the Session D-Bus, while the STT engine sits on the system D-Bus.

In general nothing needs to run as root.

The Skills Interpreter can run any configured script as the session user, so it needs to check the
ownership and permissions of the scripts and configuration.


Use of external STT engines, especially ones that make use of cloud services, have extra security and
privacy considerations.
