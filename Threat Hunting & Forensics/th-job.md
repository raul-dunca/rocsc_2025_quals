<img src="https://github.com/raul-dunca/rocsc_2025_quals/blob/main/.assets/th-job_descripiton.png">

**Q2. What is the sub Technique for the behaviour?**

For this question I didn’t use the logs, but based on the first question I googled: "sub technique for wireless attacks" and the first result was the answer.

`T1016.002`

**Q3. What is the encoding of the argument of print function of powershell? (NOT the plaintext.)**

In the KQL search query I used:

```txt
process.name : "powershell.exe" and process.command_line: *
```

And looked for the `process.command_line` field in the returned logs and found a script that encoded ”Hey, Atomic!” in base64, so I manually encoded it to get the flag.

`SGV5LCBBdG9taWMh`

**Q4. Which Ireland geolocated destination ip address is the 21th based on descending count of records?**

In the KQL search query I used:

```txt
destination.geo.country_name: "Ireland"
```

And I added `destination.ip` to the column list and clicked on `Field statistics` and then on the `Actions Icon` for `destination.ip`. There I set the number of values to 22 to see the necessary ip.

`20.191.45.158`