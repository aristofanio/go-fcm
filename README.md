# go-fcm

Firebase Cloud Messaging ( FCM ) Library using golang ( Go )

This library uses HTTP/JSON Firebase Cloud Messaging connection server protocol

This project was based in [https://github.com/douglasmakey/go-fcm](https://github.com/douglasmakey/go-fcm)


## Usage

```
go get github.com/aristofanio/go-fcm
```

## Docs

####  Firebase Cloud Messaging HTTP Protocol Specs
```
https://firebase.google.com/docs/cloud-messaging/http-server-ref
```

#### Firebase Cloud Messaging Developer docs
```
https://firebase.google.com/docs/cloud-messaging/
```

## Example

In your server:

```go
//... omitted codes

func main() {
	// init client
	client := fcm.NewClient("ApiKey")
	
	data := map[string]interface{}{
		"message": "From Go-FCM",
		"details": map[string]string{
			"name": "Name",
			"user": "Admin",
			"thing": "none",
		},
	}
	
	// use PushMultiple
	client.PushMultiple([]string{"token 1", "token 2"}, data)
	
	// registrationIds remove and return map of invalid tokens
	badRegistrations := client.CleanRegistrationIds()
	log.Println(badRegistrations) 
	
	status, err := client.Send()
	if err != nil {
		log.Fatalf("error: %v", err)
	}
	
	log.Println(status.Results)
}

//... omitted codes

```

In appengine:

```go
//... omitted codes

func handlePost(w http.ResponseWriter, r *http.Request) {

	// init client
	client := fcm.NewClient("ApiKey")
	
	// set httpClient like descript in appengine doc
	ctx := appengine.NewContext(r)
	httpClient := urlfetch.Client(ctx)
	client.SetHTTPClient(httpClient)
	
	//set data
	data := map[string]interface{}{
		"message": "From Go-FCM",
		"details": map[string]string{
			"name": "Name",
			"user": "Admin",
			"thing": "none",
		},
	}
	
	// use PushSingle
	client.PushSingle("token 1", data)
	
	status, err := client.Send()
	if err != nil {
		log.Fatalf("error: %v", err)
	}
	
	log.Println(status.Results)
}

//... omitted codes

```
