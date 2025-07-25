# UniDrop Go Client

Creating a new unidrop client to dispatch messages
```go
client := unibus.New()
```

Publishing a message within the microservice is a two (2) step process.

1. Create the message event you want to publish.
2. Call the publish method on the client to send the message.


```
event := unibus.NewEvent("company-api", "company.create", "company.create")
event.Payload = map[string]any{
    "company": map[string]any{
        "id":                  "bb4ef24b-1699-4452-ad09-f284e57c6049",
        "name":                "Acme Corporation",
        "email":               "contact@acme.com",
        "phone_number":        "+1234567890",
        "tin_number":          "1234567890",
        "registration_number": "ACME123",
        "currency":            "USD",
        "category_id":         "123",
    },
}
event.Metadata = map[string]any{
    "triggered_by": "bb4ef24b-1699-4452-ad09-f284e57c6049",
}

err := client.Publish("unidrop.company", event)
if err != nil {
    log.Fatalln("failed to send message: |", err)
}
```

Consuming message events from any microservice

```
messages, err := client.Consume("unidrop.company")
if err != nil {
    log.Println("failed to consume messages :::::: |", err)
}

for message := range messages {
    fmt.Println("Message received :::::: |", message)
}
```