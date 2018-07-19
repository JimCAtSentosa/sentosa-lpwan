# Create Company

> As far as I can tell, Things does not have the concept of company

# Create User

> Via the Things portal (did not find API to create user)

[jec] Did not find anything either.  This will likely just be a general username password to access the network.

* User Name -> `lpwan:users:username`
* Email -> `lpwan:users:email`
* Password -> `lpwan:users:passwordHash`

# Create Application

> Account Server API

```
POST /applications

[jec] code I am seeing says create an ID with POST, and PUT to add everything else.
https://www.thethingsnetwork.org/docs/applications/manager/api.html#registerapplication
https://www.thethingsnetwork.org/docs/applications/manager/api.html#setapplication 

{
  id: '' -> **?**  [jec] might as well use lpwan:applications:id
  name: String `lpwan:applications:name`
  euis: [String] -> **?**   // list of euis app is reachable at [jec] OK, weird thing: appEUI WAS a thing in LoraOpenSource
                            //                                        but was dropped as not really needed.  We may need to
                            //                                        add that back in to the applicationNetworkTypeLinks.
  rights: [String] -> **?** //  list of rights current user has for the app (which user?) [jec] don't know what this is
  collaborators: [{ -> **?** [jec] nor this
    username: String
    email: String
    rights: [String] # list of rights collaborator has to the applicaiton
  }]
  access_keys: [{ -> **?** [jec] nor this.  Though there was something about access keys to access data by api (not mqtt)
    name: String
    key: String
    rights: [String] # the rights the key will grant if used
  }]
  created: Date-Time 
  deleted: Date-Time # only if app was deleted
}
```

# Create Device Profile

> As far as I can tell, Things does not have the concept of device profile


# Create Device

> Application Manager API

```
POST /applications/{app_id}/devices
  altitude: Num -> **new**  // is this optional?
  latitude: Num -> **new**  // is this optional?
  longitude: Num -> **new** // is this optional?
  app_id: -> **?**          // where does this come from?
  attributes: { key: '', value: '' } // Assuming we don't have to do anything here
  description: String -> `lpwan:devices:description`,
  dev_id: String -> **?**
  lorawan_device: {
    activation_constraints: "e.g. local", -> **?**
    app_eui: String -> `deviceNetworkTypeLinks:networkSettings:{devEUI}`
    app_id: String -> **?** // Where does this come from?
    app_key: String -> **?**
    app_s_key: String -> `deviceNetworkTypeLinks:networkSettings:{appSKey}`,
    nwk_s_key: String -> `deviceNetworkTypeLinks:networkSettings:{nwkSKey}`
    dev_addr: String -> `deviceNetworkTypeLinks:networkSettings:{devAddr}`,
    dev_eui: `deviceNetworkTypeLinks:networkSettings:{devEUI}`,
    dev_id: String -> **?** // Where does this come from?
    disable_f_cnt_check: Bool -> `deviceNetworkTypeLinks:networkSettings:{skipFCntCheck}`
    f_cnt_down: Num -> `deviceNetworkTypeLinks:networkSettings:{fCntUp}`
    f_cnt_up: Num -> `deviceNetworkTypeLinks:networkSettings:{fCntDown}`
    last_seen: Num -> **?**
    uses32_bit_f_cnt: Bool -> **?**
  }
}
```
