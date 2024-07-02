# Stud.IP SimpleSamlPHP auth plugin

This authentication plugin enables use of SimpleSamlPHP as SSO provider.

## Prerequisites

- Stud.IP
- SimpleSamlPHP

## Installation

Put file `StudipAuthSimpleSamlPHP.class.php` into `lib/classes/auth_plugins/` directory in Stud.IP, and enable it in Stud.IP configuration by adding following line `$STUDIP_AUTH_PLUGIN[] = "SimpleSamlPHP";` to it.
Logout function is not supported for auth plugins in Stud.IP. For this reason there is updated `logout.php` class that adds this functionality to this plugin. It is needed to replace file `public/logout.php` in Stud.IP with `logout.php` that comes with this plugin.

This plugin also assumes that SimpleSamlPHP is installed in default directory `/var/simplesamlphp`.

## Configuration

This plugin is configured same way as any other authentication plugin in Stud.IP, explanation of this configuration is available in `config/config_defaults.inc.php` in Stud.IP.
There are some additional variables that are needed to be filled in. These variables are:


- return_to_url - to which URL should user be redirected after successful login
- sp_name - name of the service provider in SimpleSamlPHP configuration
- username_attribute - in which attribute is username located (if left empty it will use NameID instead)

There are also functions for user_data_mappings.


- getUserData - function for attributes that are not send in arrays.

### MockSAML config

Following configuration allows use of MockSAML idp ((https://mocksaml.com/)), for quick testing.

```
$STUDIP_AUTH_CONFIG_SIMPLESAMLPHP = array(
            "return_to_url" => 'https://studip.ceskar.xyz/index.php?sso=simplesamlphp&cancel_login=1',
            "sp_name" => 'default-sp',
            "user_data_mapping" =>      array(
                                                "auth_user_md5.Email" => array("callback" => "getUserData", "map_args" => "email"),
                                                "auth_user_md5.Nachname" => array("callback" => "getUserData", "map_args" => "firstName"),
                                                "auth_user_md5.Vorname" => array("callback" => "getUserData", "map_args" => "lastName")));
     }

```

