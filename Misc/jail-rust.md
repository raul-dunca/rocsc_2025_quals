Seeing that many people solved this challenge, I gave it a shot, despite not being too familiar with rust. By providing no input, I discovered that a function called `fun` gets executed, but it was not defined. This means that I must define it and somehow read the flag. I tried different approaches, but nothing worked out, so I googled: "rust jail ctf", just to see similar challenges and get inspired. I found a github: https://gist.github.com/domenukk/7f9b353d2887d1b98fe370ff254069d7 which contains a bash script which looked very similar to how the server works. And given that the name of the CTF is in the title, I googled "HackIM CTF 2023 writeups funsafe" and found:
https://ctftime.org/writeup/37804
Used the payload and it worked.

`CTF{f666ded66f578ceaa00a2ac4f2f9b8f5d393f75c5fd1e8cdd5dbbd8a057fa19c}`
