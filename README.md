

# Example if you'd like to use **render** for [gscloud](https://github.com/gridscale/gscloud)

```go
func main(){
  config := gsclient.NewConfiguration(
		"YOUR-API-URL",
		"YOUR-UUID",
		"YOUR-API-TOKEN",
		false, //Set debug mode
		true,  //Set sync mode
		500,   //Delay (in milliseconds) between requests
		100,   //Maximum number of retries
	)
	client := gsclient.NewClient(config)
	ctx := context.Background()

	out := new(bytes.Buffer)
```
### List Servers
___

```go
	servers, _ := client.GetServerList(ctx)
	var serverinfos [][]string
	for _, server := range servers {

		fill := [][]string{
			{
				server.Properties.Name,
				strconv.FormatInt(int64(server.Properties.Cores), 10),
				strconv.FormatInt(int64(server.Properties.Memory), 10),
				server.Properties.Status,
			},
		}
		serverinfos = append(serverinfos, fill...)

	}
```
### List Networks
___

```go

	network, _ := client.GetNetworkList(ctx)
	var networkinfos [][]string
	for _, netw := range network {
		fill := [][]string{
			{
				netw.Properties.Name,
				netw.Properties.LocationCountry,
				netw.Properties.NetworkType,
				netw.Properties.Status,
			},
		}
		networkinfos = append(networkinfos, fill...)
	}
```
### List Storage
___

```go

	storage, _ := client.GetStorageList(ctx)
	var storageinfos [][]string
	for _, stor := range storage {

		fill := [][]string{

			{
				stor.Properties.Name,
				strconv.FormatInt(int64(stor.Properties.Capacity), 10),
				stor.Properties.StorageType,
				stor.Properties.Status,
			},
		}
		storageinfos = append(storageinfos, fill...)
	}
```
### Print everything
___

```go
	// Printing Tables
	render.Table(out, []string{"server-name", "cores", "memory", "status"}, serverinfos)
	render.Table(out, []string{"storage-name", "capacity", "storage-type", "status"}, storageinfos)
	render.Table(out, []string{"network-name", "location", "network-type", "status"}, networkinfos)
	fmt.Print(out)

	// Printing JSON
	render.AsJSON(out, servers, network, storage)  // You can add slices variadic to out
	fmt.Print(out)
}	

```

