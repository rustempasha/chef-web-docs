---
resource_reference: true
resources_common_guards: true
resources_common_notification: true
resources_common_properties: true
title: osx_profile resource
resource: osx_profile
aliases:
- "/resource_osx_profile.html"
menu:
  infra:
    title: osx_profile
    identifier: chef_infra/cookbook_reference/resources/osx_profile osx_profile
    parent: chef_infra/cookbook_reference/resources
resource_description_list:
- markdown: Use the **osx_profile** resource to manage configuration profiles (`.mobileconfig`
    files) on the macOS platform. The **osx_profile** resource installs profiles by
    using the uuidgen library to generate a unique `ProfileUUID`, and then using the
    `profiles` command to install the profile on the system.
resource_new_in: '12.7'
syntax_full_code_block: |-
  osx_profile 'name' do
    identifier        String
    profile           String, Hash
    profile_name      String # default value: 'name' unless specified
    action            Symbol # defaults to :install if not specified
  end
syntax_properties_list:
syntax_full_properties_list:
- "`osx_profile` is the resource."
- "`name` is the name given to the resource block."
- "`action` identifies which steps Chef Infra Client will take to bring the node into
  the desired state."
- "`identifier`, `profile`, and `profile_name` are the properties available to this
  resource."
actions_list:
  :install:
    markdown: Default. Install the specified configuration profile.
  :nothing:
    shortcode: resources_common_actions_nothing.md
  :remove:
    markdown: Remove the specified configuration profile.
properties_list:
- property: identifier
  ruby_type: String
  required: false
  description_list:
  - markdown: Use to specify the identifier for the profile, such as `com.company.screensaver`.
- property: path
  ruby_type: String
  required: false
  default_value: null
  new_in: null
  description_list:
  - markdown: The path to write the profile to disk before loading it.
- property: profile
  ruby_type: String, Hash
  required: false
  description_list:
  - markdown: Use to specify a profile. This may be the name of a profile contained
      in a cookbook or a Hash that contains the contents of the profile.
- property: profile_name
  ruby_type: String
  required: false
  default_value: The resource block's name
  description_list:
  - markdown: Use to specify the name of the profile, if different from the name of
      the resource block.
examples: |
  **Install a profile from a cookbook file**

  ```ruby
  osx_profile 'com.company.screensaver.mobileconfig'
  ```

  **Install profile from a hash**

  ```ruby
  profile_hash = {
    'PayloadIdentifier' => 'com.company.screensaver',
    'PayloadRemovalDisallowed' => false,
    'PayloadScope' => 'System',
    'PayloadType' => 'Configuration',
    'PayloadUUID' => '1781fbec-3325-565f-9022-8aa28135c3cc',
    'PayloadOrganization' => 'Chef',
    'PayloadVersion' => 1,
    'PayloadDisplayName' => 'Screensaver Settings',
    'PayloadContent'=> [
      {
        'PayloadType' => 'com.apple.ManagedClient.preferences',
        'PayloadVersion' => 1,
        'PayloadIdentifier' => 'com.company.screensaver',
        'PayloadUUID' => '73fc30e0-1e57-0131-c32d-000c2944c108',
        'PayloadEnabled' => true,
        'PayloadDisplayName' => 'com.apple.screensaver',
        'PayloadContent' => {
          'com.apple.screensaver' => {
            'Forced' => [
              {
                'mcx_preference_settings' => {
                  'idleTime' => 0,
                }
              }
            ]
          }
        }
      }
    ]
  }

  osx_profile 'Install screensaver profile' do
    profile profile_hash
  end
  ```

  **Remove profile using identifier in resource name**

  ```ruby
  osx_profile 'com.company.screensaver' do
    action :remove
  end
  ```

  **Remove profile by identifier and user friendly resource name**

  ```ruby
  osx_profile 'Remove screensaver profile' do
    identifier 'com.company.screensaver'
    action :remove
  end
  ```
---