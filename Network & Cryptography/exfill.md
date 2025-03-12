<img src="https://github.com/raul-dunca/rocsc_2025_quals/blob/main/.assets/exfill_description.png">

In the pcapng file, I saw multiple `GET` requests, so I put 2 in [diffchecker](https://www.diffchecker.com/text-compare/) to see what is different. There I noticed that the `User-Agent` is different and that they contain some weird zeros, which seemed odd. Given the content of the pages, I googled "Chuck Noris encoding" and found: https://www.dcode.fr/chuck-norris-code
Then I extracted all the `User-Agents` values from the pcapng using:

```bash
tshark -r captura.pcapng -Y 'http.user_agent contains "curl/"' -T fields -e http.user_agent > user_agents.txt
```

And now in `user_agents.txt` I had all the `User-Agents`, including the `curl/` part which I didn’t need. I created a python script to concatenate the zeros and get rid of the unnecessary part:

```python
with open("user_agents.txt") as f:
    user_agents=[line.rstrip('\n') for line in f]

chuck_noris=""
for agent in user_agents:
    agent=agent.replace("curl/", "")
    chuck_noris+=agent

print(chuck_noris)
```

I then used the output of my script as input at: https://www.dcode.fr/chuck-norris-code
The output was a string containing only `C` and `N`. After attempting to decode this with no success, I bought a hint which suggested that decabit code was used. So, I created another small program to map `C` to `–` and `N` to `+`:

```python
string="CCCCNNN…"
out=""
for i in string:
    if i=='C':
        out+="-"
    else:
        out+="+"
print(out)
```

Decoded the output using: https://www.dcode.fr/decabit-code, which returns the flag in reverse order.

`ctf{b3d7630e73726a79f39210a8c5e170aa1da595404aacbf0c765501c8c3257e5b}`
