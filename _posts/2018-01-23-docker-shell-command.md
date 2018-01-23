---
layout: post
title:  "Docker Shell Command"
author: "Matthew"
---

After adding docker to my utility belt, I haven't looked back. One thing i often find myself needing to do is execute a bash session against my container.

What this looks like as docker command is:

{% highlight bash %}
docker exec -it <container> /bin/bash
{% endhighlight %}

In order to save key strokes i added it to my `~/.bashrc`.

{% highlight bash %}
docker-shell() {
  docker exec -it $1 /bin/bash
}
{% endhighlight %}

Now we have our command we can add tab completion for the container name argument:

{% assign special = '{{.Names}}' %}

{% highlight bash %}
_docker_shell()
{
  local procs
  _get_comp_words_by_ref -n : cur
  procs="$(docker ps -a --format '{{ special }}')"
  COMPREPLY=( $(compgen -W "${procs}" -- ${cur}) )
  return 0
}

complete -F _docker_shell docker-shell
{% endhighlight %}

Again updating my `~/.bashrc` with the following line, where the part after the `.` is the location of the above script:

{% highlight bash %}
. /usr/share/bash-completion/completions/docker-shell
{% endhighlight %}

Now just a tab away from accessing the containers:

{% highlight bash %}
$ docker-shell m
mcwarman.github.io       mcwarman.github.io_blog  mysql
{% endhighlight %}

NOTE: With Git for Windows or Cygwin you'll prefix the `docker exec` with `winpty`.