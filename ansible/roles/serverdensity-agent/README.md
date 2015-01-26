ServerDensity Agent Playbook
----------------------------

To install the agent:

* define your server density credentials in your `vars` file
* edit templates/server-density-agent.cfg.j2 to customize your monitoring agent setup *
run the playbook on your selected servers

* Tip: You can create variables for the custom settings you add to server-density-agent.cfg.j2 (e.g. MySQL / MongoDB) if the configuration varies by server.

* Copied from
  [https://github.com/daviddoran/ansible-patterns/tree/master/server-density-agent](https://github.com/daviddoran/ansible-patterns/tree/master/server-density-agent)
