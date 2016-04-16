# Intro: #
This is an Erlang Powered linux command line tool.<br>
You can use this tool to connect to mass of servers through SSH client simultaneously and parallel execute noninteractive shell commands on every connected server.<br>
<br>
<h1>Requirements:</h1>
<ul><li>1.openssh client with agent foward enabled(on windows putty or secureCRT also support agent forward)<br>
</li><li>2.GNU lib readline(libreadline.so,libhistory.so)<br>
</li><li>3.Erlang 5.6/OTP R12B</li></ul>


<h1>Demo:</h1>
The full command is<br>
<pre><code>erl -noinput -pa $ERLYSSH_ROOT/ebin -run essh start $PATH_TO_LD_LIBRARY_PATH $path_to_server_list_file<br>
</code></pre>
You can write your shell wrapper to simplify the shell command. I made a dummy wrap that can start erlyssh with:<br>
<pre><code>radiumce@app-88:~$ essh cometd                                             <br>
-----------------www-11 connected-------------------                    <br>
-----------------www-15 connected-------------------                    <br>
                                                                        <br>
essh&gt;:       <br>
</code></pre>
'essh' is the shell command for elyssh <br>
'cometd' is the name of server_list_file. The file's format is<br>
<pre><code>www-11<br>
www-15<br>
</code></pre>
just each server address a line.(you can comment a server with #)<br>
<br>
After the 'essh>:'  prompt, you can run any non-interactive commands.<br>
for exmaple:<br>
<pre><code>radiumce@app-88:~$ essh cometd<br>
-----------------www-11 connected-------------------<br>
-----------------www-15 connected-------------------<br>
<br>
essh&gt;: ls<br>
--------------www-11---------------<br>
erlang<br>
<br>
<br>
[primary server done]<br>
-&gt;&gt;<br>
<br>
essh&gt;:<br>
</code></pre>
Input a 'ls' command, and it will parallel executed on www-11,www-15.<br>
In erlyssh the first server on the list will be the primary server. Its output is <br>
realtime(it is useful when running some long duration commands, such as 'svn up').<br>
when the command on www-15 is done, it will compare with the out put of www-11.<br>
when their output is identical, erlssh just print a '->>' as www-15's output.<br>

<pre><code>essh&gt;: #con 5<br>
set concurrent execution limits = 5<br>
essh&gt;: <br>
</code></pre>
Schedule parameter 'con'(parallel connection limit, default 256) can be set by '#con number'<br>
<br>
<br>
<pre><code>essh&gt;: #intv 5<br>
set execution interval = 5s<br>
essh&gt;: <br>
</code></pre>
Schedule parameter 'intv'(execution interval, default 0) can be set by '#intv seconds'<br>
<br>
<pre><code>essh&gt;: exit;<br>
Thanks for using essh, bye.<br>
</code></pre>
Use command 'exit;'(end with semi) to exit the shell.<br>
<br>
<br>
<br>
<h1>Tips</h1>
1. erlyssh is interactive(you can use 'cd' to change path, so it's interactive) but it can only execute non-interactive commands.<br>
2. When there is mass of servers, add<br>
<pre><code>   CheckHostIP no<br>
   StrictHostKeyChecking no<br>
</code></pre>
<blockquote>to your .ssh/config can avoid some yes/no security confirm.<br>
3. Many shell commands have their non-interactive mode or corresponded non-interactive commands. Such as 'svn' commands have non-interactive mode and 'sed' can help you to perform plain text processing.