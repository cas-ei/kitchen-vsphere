# <a name="title"></a> Kitchen::Vsphere: A Test Kitchen Driver for VMWare vSphere

A [Test Kitchen](https://kitchen.ci/) Driver for VMWare vSphere

This driver uses the [fog gem](https://rubygems.org/gems/fog) to provision and destroy VMWare vSphere instances. Use your own vSphere host for your infrastructure testing!

Shamelessly copied from [Fletcher Nichol](https://github.com/fnichol)'s
awesome work on an [EC2 driver](https://github.com/opscode/kitchen-ec2).

## Installation

Add this line to your application's Gemfile:

    gem 'kitchen-vsphere'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install kitchen-vsphere

## Usage

Provide, at a minimum, the required driver options in your `.kitchen.yml` file:

    driver_plugin: vsphere
    driver_config:
      username: [ssh user name of provisioned guest (default is root)]
      password: [ssh password of provisioned guest]
      authentication:
        vsphere_username: [your vsphere server user name]
        vsphere_password: [your vsphere server password]
        vsphere_server: [your vsphere server host name]
        vsphere_expected_pubkey_hash: [hash of your hosts public ssl key]
      server_create:
        datacenter: [vsphere datacenter name where template is located]
        network_label: [vsphere network name to use]
        network_adapter_device_key: [network key]
        template_path: [path to the template of vm to clone]

The `template-path` option can be specified as a template name or vm name. The path should include the folder path relative to the `datacenter`.

By default, a unique server name will be generated and the current user's RSA SSH key will be used.

## Finding the required expected hash
If you need the expected hash for your vsphere instance you can use the fog gem directly
```bash
#install the fog gem
gem install fog
```
then launch the interactive ruby console and fire off the following:
```ruby
require 'fog'
Fog::Compute.new(:provider => "vsphere", :vsphere_username => "VSPHERE_USERNAME", :vsphere_password=> "PASSWORD", :vsphere_server => "HOSTNAME")
```

which will respond with the following message containing your server public key hash:
```
Fog::Vsphere::Errors::SecurityError: The remote system presented a public key with hash 74e6f3f9a9d50be352aa0fabcdc1df9977016af38da538cf76b3ba56a6363d11 but we're expecting a hash of <unset>...
```
## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Run style checks and RSpec tests (`bundle exec rake`)
4. Commit your changes (`git commit -am 'Add some feature'`)
5. Push to the branch (`git push origin my-new-feature`)
6. Create new Pull Request
