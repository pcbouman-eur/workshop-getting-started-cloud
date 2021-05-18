---
title: Useful Commands
---

<table>
    <thead>
        <th scope="col" style="width: 17em">Command</th>
        <th scope="col">Action</th>
    </thead>
    <tbody>
        <tr>
            <td><code>whoami</code></td>
            <td>Print your username</td>
        </tr>
        <tr>
            <td><code>pwd</code></td>
            <td>Print current working directory</td>
        </tr>
        <tr>
            <td><code>cd name</code></td>
            <td>Move to directory called <code>name</code></td>
        </tr>									
        <tr>
            <td><code>cd ..</code></td>
            <td>Move up one directory</td>
        </tr>	
        <tr>
            <td><code>mkdir name</code></td>
            <td>Creates a directory called <code>name</code></td>
        </tr>
        <tr>
            <td><code>ls</code></td>
            <td>List files in current working directory</td>
        </tr>
        <tr>
            <td><code>ls -l</code></td>
            <td>List files in current working directory (long format)</td>
        </tr>
        <tr>
            <td><code>ls --help</code></td>
            <td>Help for the <code>ls</code> command</td>
        </tr>
        <tr>
            <td><code>man ls</code></td>
            <td>Read the manual of the <code>ls</code> command. Quit by pressing <kbd>Q</kbd></td>
        </tr>
        <tr>
            <td><code>tree -L 2</code></td>
            <td>Shows a file and directory tree of depth 2.</td>
        </tr>
        <tr>
            <td><code>nano file</code></td>
            <td>Opens a file called <code>file</code> in the nano editor. <br /> Save your edits with <kbd>Ctrl</kbd>+<kbd>O</kbd>
            and quit nano with <kbd>Ctrl</kbd>+<kbd>X</kbd></td>
        </tr>
        <tr>
            <td><code>cat file</code></td>
            <td>Print the contents of the file called <code>name</code> to the terminal</td>
        </tr>
        <tr>
            <td><code>mv from to</code></td>
            <td>Move or rename a file called <code>from</code> to new location or name <code>to</code></td>
        </tr>									
        <tr>
            <td><code>rm file</code></td>
            <td>Delete a file called <code>name</code></td>
        </tr>	
        <tr>
            <td><code>rm -rf dir</code></td>
            <td>Delete director called <code>dir</code> and all files and subdirectories it contains. <br /><strong>Be very careful with this!</strong></td>
        </tr>
        <tr>
            <td><code>wget url</code></td>
            <td>Download the file specified by URL <code>url</code> and save it as a file</td>
        </tr>
        <tr>
            <td><code>head -n 5 file</code></td>
            <td>Shows the first 5 lines of <code>file</code></td>
        </tr>
        <tr>
            <td><code>tail -n 5 file</code></td>
            <td>Shows the first 5 lines of <code>file</code></td>
        </tr>
        <tr>
            <td><code>less file</code></td>
            <td>Read <code>file</code> interactively. Exit by pressing <kbd>Q</kbd></td>
        </tr>
        <tr>
            <td><code>python3 file</code></td>
            <td>Run the python code in <code>file</code> with the Python interpreter </td>
        </tr>
        <tr>
            <td><code>Rscript file</code></td>
            <td>Run the R code in <code>file</code> with the R interpreter</td>
        </tr>
        <tr>
            <td><code>javac files</code></td>
            <td>Compile one or multiple <code>.java</code> source code <code>files</code> to <code>.class</code> bytecode files.</td>
        </tr>									
        <tr>
            <td><code>java ClassName</code></td>
            <td>Run the program starting from the main method in class <code>ClassName</code></td>
        </tr>
        <tr>
            <td><code>command &lt; file</code></td>
            <td>Runs <code>command</code> and sends the contents of <code>file</code> to the standard in of that process as if someone was typing it into the terminal while that process is running.</td>
        </tr>
        <tr>
            <td><code>crontab -e</code></td>
            <td>Edit your user's cron table that specifies scheduled tasks</td>
        </tr>
        <tr>
            <td><code>date</code></td>
            <td>Prints the current date and time</td>
        </tr>									
        <tr>
            <td><code>tmux new -s name</code></td>
            <td>Create a new virtual terminal session with name <code>name</code></td>
        </tr>
        <tr>
            <td><code>tmux</code></td>
            <td>Create a new virtual terminal session</td>
        </tr>
        <tr>
            <td><code>tmux ls</code></td>
            <td>List existing <code>tmux</code> virtual terminal sessions</td>
        </tr>									
        <tr>
            <td><code>tmux detach</code> <br />
                or press <kbd>Ctrl</kbd> + <kbd>B</kbd>, then <kbd>D</kbd></td>
            <td>Detach from the current virtual terminal</td>
        </tr>
        <tr>
            <td><code>tmux attach -t name</code></td>
            <td>Enter the virtual terminal sessions called <code>name</code></td>
        </tr>
        <tr>
            <td><code>tmux kill-session -t name</code></td>
            <td>Ends the virtual terminal session called <code>name</code>. <br />
            This ends all programs running in it.</td>
        </tr>
        <tr>
            <td><code>exit</code></td>
            <td>Leaves the virtual terminal (if inside one). <br /> Closes the connection if you
                perform it in the shell directly.
            </td>
        </tr>
        <tr>
            <td><code>htop</code></td>
            <td>Check resource consumption. <br /> Press <kbd>F10</kbd> or click <code>F10Quit</code> to exit.
            </td>
        </tr>
    </tbody>
</table>

{% include links.md %}
