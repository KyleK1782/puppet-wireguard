# puppet-wireguard

#### Table of Contents

1. [Overview](#overview)
2. [Usage - Configuration options and additional functionality](#usage)
3. [Development - Guide for contributing to the module](#development)

## Overview

This module installs and configures Wireguard. 

## Usage

The basic usage is:

(1 to 1 connection)
* This is on the 1st VM *
```puppet
wireguard::iface { 'wg-example0':
  addresses    => ['192.168.1.1'],
  listenport   => $wg_port,
  collect_tags => ['examplepeers'], # where to select our peers from using this tage (i.e. from the 2nd vm)
}
wireguard::peer { 'wg-example0':
  allowedips          => ['192.168.1.1/24'],
  endpoint            => "${::networking['ip']}:${wg_port}",
  persistentkeepalive => 120,
  export_tags         => ['outpeers'], # Where the peer should be placed exported to ::iface using this tag
}
```


* This is on the 2nd VM *
```puppet
wireguard::iface { 'wg-example0':
  addresses    => ['192.168.1.1o'],
  listenport   => $wg_port,
  collect_tags => ['outpeers'], # where to select our peers from, in this case from the 1st VM
}
wireguard::peer { 'wg-example0':
  allowedips          => ['192.168.1.1/24'],
  endpoint            => "192.168.1.10",
  persistentkeepalive => 120,
  export_tags         => ['examplepeers'], # Where the peer should be placed exported to ::iface (i.e. on the 1st VM)
}
```

# Development
