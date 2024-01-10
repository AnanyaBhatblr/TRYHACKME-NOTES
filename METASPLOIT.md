The Metasploit Framework is a set of tools that allow information gathering, scanning, exploitation, exploit development, post-exploitation, and more.
components of metasploit framework-
- msfconsole
- modules
- tools

note: payloads are code that will run on the target system

## PAYLOADS
- **Adapters:** An adapter wraps single payloads to convert them into different formats. For example, a normal single payload can be wrapped inside a Powershell adapter, which will make a single powershell command that will execute the payload.  
- **Singles:** Self-contained payloads (add user, launch notepad.exe, etc.) that do not need to download an additional component to run.
- **Stagers:** Responsible for setting up a connection channel between Metasploit and the target system. Useful when working with staged payloads. “Staged payloads” will first upload a stager on the target system then download the rest of the payload (stage). This provides some advantages as the initial size of the payload will be relatively small compared to the full payload sent at once.
- **Stages:** Downloaded by the stager. This will allow you to use larger sized payloads.

Metasploit has a subtle way to help you identify single (also called “inline”) payloads and staged payloads.

- generic/shell_reverse_tcp
- windows/x64/shell/reverse_tcp

Both are reverse Windows shells. The former is an inline (or single) payload, as indicated by the `_` between “shell” and “reverse”. While the latter is a staged payload, as indicated by the `/` between “shell” and “reverse”.

[https://github.com/rapid7/metasploit-framework/wiki/Exploit-Ranking](https://github.com/rapid7/metasploit-framework/wiki/Exploit-Ranking)]

commands:
setg
unsetg
back
exploit -z
exploit

