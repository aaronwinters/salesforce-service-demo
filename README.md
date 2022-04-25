# demo-salesforce-servicecloud

Sample customer service solution using Salesforce Service Cloud. Used for feature demos and proofs of concept.

Key features include agent service console, omni-channel routing and omni-channel supervisor experience.

There are two tooling options for contributing to this project - using the Salesforce CLI, or using CumulusCI.
# Develop using CumulusCI
## Set up
1. [Install Salesforce CLI](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)
1. [Set up CumulusCI](https://cumulusci.readthedocs.io/en/latest/tutorial.html)
## Development

To work on this project in a scratch org:

1. Create feature branch `git checkout -b feature/descriptiveBranchName`
1. Run `cci flow run dev_org --org dev` to create a new scratch org and deploy this project.
1. Run `cci org browser dev` to open the org in your browser.
1. After developing feature in scratch org run `cci task run list_changes --org dev` to see changes that will sync
1. Run `cci task run retrieve_changes --org dev -o exclude "Profile:"` to retrieve the changes locally, except profles - WGU avoids using profiles where possible
1. If developing locally, run `cci task run dx_push --org dev` to push local changes to scratch org
1. Push to GitHub and create a pull request for review
1. Delete the scratch org once feature is accepted `cci org scratch_delete dev`

## Test Data
Test data is typically loaded automatically as part of the scratch org build process. To manually load test data run
```
cci task run snowfakery --receipe datasets/start_data.yml --org dev
```

# Develop using the Salesforce CLI
## Set up
1. [Install and set up the Salesforce CLI](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)
1. Authorize a DevHub `sfdx auth:web:login --setdefaultdevhubusername --setalias my-hub-org`

## Development
1. Create feature branch `git checkout -b feature/descriptiveBranchName`
1. Create scratch org with an alias (service-demo-scratch) `sfdx force:org:create -s -f orgs/dev.json -a service-demo-scratch`
1. Push the app to scratch org `sfdx force:source:push`
1. Assign the permission set group to the default user `sfdx force:user:permset:assign -n "Customer_Support_Console_User"`
1. Develop locally or in scratch org and use `sfdx force:source:push` and `sfdx force:source:pull` to sync changes

### Manual setup steps for scratch org setup
1. Add Service Presence Status to Admin profile - Navigate to Admin profile. Hover over over Enabled Service Presence Status Access and click Edit. Select and add the statuses.
1. Add user to Tier1 Agents Presence Configuration - this handled already for sys admin users by assigning the profile, but needs to be done for other users and/or profiles.

## Prepare Demo Environment
1. Run `cci flow run qa_org --org demo` to set up a scratch org named "demo" with 30 days lifespan.
1. Update Service Presence Statuses and Presence Configuration as necessary (see manual steps above).
1. Load demo data `cci task run generate_and_load_from_yaml -o generator_yaml datasets/start_data.yml --org dev`

# Resources
[Omni-channel Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.230.0.omni_channel_dev.meta/omni_channel_dev/omnichannel_developer_guide_intro.htm)