![[Pasted image 20250119171312.png]]

>[!tip]
>1C == 00011100
>Flipping the 7th bit gives us : 0001 1110 -> 1E

![[Pasted image 20250119171946.png]]

![[Pasted image 20250119172755.png]]

![[Pasted image 20250119173434.png]]

>[! important]
>**192.168.0.100 Locale Interne**
>**209.165.20.25 Globale Interne**

![[Pasted image 20250119174952.png]]

![[Pasted image 20250119180221.png]]

![[Pasted image 20250119180420.png]]

![[Pasted image 20250119181215.png]]

![[Pasted image 20250119181303.png]]

![[Pasted image 20250119181801.png]]

![[Pasted image 20250119181849.png]]

![[Pasted image 20250119182240.png]]

```shell
CiscoIOS#> access-list 101 permit tcp host 193.48.29.2 any eq 23
CiscoIOS#> access-list 101 permit tcp 193.48.29.4 0.0.0.0 any eq 23
CiscoIOS#> access-list 101 deny tcp any any eq 23
CiscoIOS#> access-list 101 permit ip any any
```

![[Pasted image 20250119182252.png]]

```shell
CiscoIOS#> access-list 102 deny UDP 193.48.29.0 0.0.0.255 any eq 135  
CiscoIOS#> access-list 102 deny UDP 193.48.29.0 0.0.0.255 any eq 139
CiscoIOS#> access-list 102 deny UDP 193.48.29.0 0.0.0.255 any eq 445

CiscoIOS#> access-list 102 deny TCP 193.48.29.0 0.0.0.255 any eq 135
CiscoIOS#> access-list 102 deny TCP 193.48.29.0 0.0.0.255 any eq 139
CiscoIOS#> access-list 102 deny TCP 193.48.29.0 0.0.0.255 any eq 445

CiscoIOS#> access-list 102 permit IP any any
```

![[Pasted image 20250119182308.png]]

```shell
CiscoIOS#> access-list 103 permit TCP any host 192.48.29.100 eq 25
CiscoIOS#> access-list 103 permit UDP any host 192.48.29.100 eq 53
CiscoIOS#> access-list 103 permit TCP any host 192.48.29.100 eq www
```

![[Pasted image 20250119182321.png]]
![[Pasted image 20250119184304.png]]

```shell

```