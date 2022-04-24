# demo-salesforce-servicecloud

Sample customer service solution using Salesforce Service Cloud. Used for feature demos and proofs of concept.

Key features include agent service console, omni-channel routing and omni-channel supervisor experience.

## Set up
1. Install the Salesforce CLI and connect to a DevHub org
1. [Set up CumulusCI](https://cumulusci.readthedocs.io/en/latest/tutorial.html) - this project uses CCI to load test and demo data into the Salesforce org. Since we have CumulusCI available, we are using the task runner functionality as well.
## Development

To work on this project in a scratch org:

1. Create feature branch `git checkout -b feature/jira-key-descriptiveBranchName`
1. Run `cci flow run dev_org --org dev` to create a new scratch org and deploy this project.
1. Run `cci task run generate_and_load_from_yaml -o generator_yaml datasets/start_data.yml --org dev` to load sample data into the scratch org
1. Run `cci org browser dev` to open the org in your browser.
1. After developing feature in scratch org run `cci task run list_changes --org dev` to see changes that will sync
1. Run `cci task run retrieve_changes --org dev -o exclude "Profile:"` to retrieve the changes locally, except profles - WGU avoids using profiles where possible
1. Push to GitHub and create a pull request for review
1. Review changes in a new scratch org `cci flow run qa_org --org qa` and `cci org browser qa`
1. Delete the scratch orgs once feature is accepted `cci org scratch_delete dev` and `cci org scratch_delete qa`

### Manual setup steps for scratch org setup
1. Add Service Presence Status to Admin profile - Navigate to Admin profile. Hover over over Enabled Service Presence Status Access and click Edit. Select and add the statuses.
1. Add user to Tier1 Agents Presence Configuration - this handled already for sys admin users by assigning the profile, but needs to be done for other users and/or profiles.

## Prepare Demo Environment
1. Run `cci flow run qa_org --org demo` to set up a scratch org named "demo" with 30 days lifespan.
1. Update Service Presence Statuses and Presence Configuration as necessary (see manual steps above).
1. Load demo data `cci task run generate_and_load_from_yaml -o generator_yaml datasets/start_data.yml --org dev`

# Resources
[Omni-channel Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.230.0.omni_channel_dev.meta/omni_channel_dev/omnichannel_developer_guide_intro.htm)