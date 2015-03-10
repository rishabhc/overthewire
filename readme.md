I recently solved the Bandit wargame(http://overthewire.org/wargames/bandit/) at OverTheWire, the primary aim of which is to give a headstart to absolute beginners in security. Although the solutions of Level 0-24 are widely available on the internet, Level 25 and 26 were added recently. They are tougher than the previous ones and have not many guides as to how to solve them. Here are hints and solutions for the same.

#Level 25
http://overthewire.org/wargames/bandit/bandit25.html

The problem does not require a hint as such as it is obvious that you have to write a script that brute forces the password. Your script should concatenate the password for level 24 and a 4 digit number(along with a space) and send it to the daemon on port 30002. If the 4 digit number matches you are given the password. My shell script for the same was this

```
for i in in {0000..9999}
do
	a="$(echo "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ $i" | nc localhost 30002 2>&1)";
	if echo $a | grep -q "Correct";
	then
		echo $a;
		break;
	else
		echo "nope";
	fi;
done
```

Remember, you have to execute this script after SSHing into bandit24. The script echos the password of bandit24 and a 4 digit number to the port 30002, as to why I included `2>&1` can be read here : http://stackoverflow.com/questions/15391254/save-result-to-variable . If the response from the port contains *Correct* I break the script and echo the response from the daemon which contains the password for the bandit25, if not I just echo a *nope*.

Turns out the correct number is `5669` and the password for the next level is `uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG`.

#Level 26
http://overthewire.org/wargames/bandit/bandit26.html

Thanks to [captn3m0](https://github.com/captn3m0/) who gave me the initial hint to solve this problem which was to run
```
cat /usr/bin/showtext
```

Further hints are given below

1. Focus on the `more` being executed
2. There is a reason why 'bandit26' written in that particular fashion spans more than 1 line
3. Did you know that `more` command had an interactive shell too?
4. Did you know you could open vi from the interactive shell?
5. Research what `:r *file_name*` in vi does

If the answer is not obvious enough from the above hints, here goes. 

1. To get in to the interactive shell of more command, the output needs to exceed the height of the terminal i.e. you need a page break. So, decrease the height of the terminal to about 3-4 lines.
2. SSH into *bandit26*. We are now able to access the interactive shell. Enter `vi` to open up an editor.
3. Type `:r /etc/bandit_pass/bandit26` and you get the password for level 26.    
   
