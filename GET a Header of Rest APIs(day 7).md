```
curl --insecure \
| --verbose \
| --user cisco:cisco \
| --header "Accept: application/yang+data+json" \
| https://10.1.1.254/restconf/data/Cisco-IOS-XE-native:native/hostname

curl --insecure \
| --user cisco:cisco \
| --header "Accept: application/yang+data+json" \
| --request GET \
| https://10.1.1.254/restconf/data/Cisco-IOS-XE-native:native/interface

curl --insecure \
| --user cisco:cisco \
| --header "Accept: application/yang+data+json" \
| --request GET \
| https://10.1.1.254/restconf/data/Cisco-IOS-XE-native:native/interface/Loopback=1/description

curl --insecure \
| --user cisco:cisco \
| --header "Accept: application/yang+data+json" \
| --header "Content-type: application/yang+data+json" \
| --request PUT \
| --data '
	{
		"Cisco=IOS-XE-native:description: "W in the chat"
	}
' \
| https://10.1.1.254/restconf/data/Cisco-IOS-XE-native:native/interface/Loopback=1/description
```