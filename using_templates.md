# Using Templates

Templates are documents that combine code, data, and literal text to produce a final rendered output. The goal of a template is to manage a complicated piece of text with simple inputs.

In Puppet, you’ll usually use templates to manage the content of configuration files (via the content attribute of the file resource type).

###Templating Languages
Templates are written in a templating language, which is specialized for generating text from data.

Puppet supports two templating languages:

* Embedded Puppet (EPP) uses Puppet expressions in special tags. It’s easy for any Puppet user to read, but only works with newer Puppet versions. (≥ 4.0, or late 3.x versions with future parser enabled.)
* Embedded Ruby (ERB) uses Ruby code in tags. You need to know a small bit of Ruby to read it, but it works with all Puppet versions.

###Using Templates
Once you have a template, you can pass it to a function that will evaluate it and return a final string.

The actual template can be either a separate file or a string value. You’ll use different functions depending on the template’s language and how the template is stored.

|Template Language	|File	|String
|--|--|--
|Embedded Puppet (EPP)	|epp	|inline_epp
|Embedded Ruby (ERB)	|template	|inline_template

###With a Template File: template and epp

You can put template files in the templates directory of a module. EPP files should have the `.epp` extension, and ERB files should have the `.erb` extension.

To use a template file, evaluate it with the template (ERB) or epp function as follows:
```
# epp(<FILE REFERENCE>, [<PARAMETER HASH>])
file { '/etc/ntp.conf':
  ensure  => file,
  content => epp('ntp/ntp.conf.epp', {'service_name' => 'xntpd', 'iburst_enable' => true}),
  # Loads /etc/puppetlabs/code/environments/production/modules/ntp/templates/ntp.conf.epp
}

# template(<FILE REFERENCE>, [<ADDITIONAL FILES>, ...])
file { '/etc/ntp.conf':
  ensure  => file,
  content => template('ntp/ntp.conf.erb'),
  # Loads /etc/puppetlabs/code/environments/production/modules/ntp/templates/ntp.conf.erb
}
```

###Referencing Files
The first argument to these functions should be a string like `'<MODULE>/<FILE>'`, which will load `<FILE>` from `<MODULE>`’s templates directory.

|File Reference	|Actual File
|--|--|
|ntp/ntp.conf.epp	|<MODULES DIRECTORY>/ntp/templates/ntp.conf.epp
|activemq/amq/activemq.xml.erb	|<MODULES DIRECTORY>/activemq/templates/amq/activemq.xml.erb