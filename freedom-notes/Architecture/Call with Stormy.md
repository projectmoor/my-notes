Package: semantic-release
will soon be used in FreedomSDK

fix(): 
feat():

perf():

-------
General rule, keep PR less than 700 lines change.

----

Leaf Node

Root Node = \_App.tsx

Branch out to AppLayout Node, further banch into features layout.
Then pages nodes. 

The last layer of nodes are called the leaf nodes.

At last layer, using useDevicesData hook can be dangerous.

We should pass in data to the component instead.


------

git reflog

git rebase -i HEAD^2 (interactive rebase)


-----
Custom LISP Language:

["$", "rewrw_id"] => 324

List type variable: variable or value.

type defined by OFL
automations_task_types: three types supported
OFL


-----
shared by Nick
https://www.kxan.com/news/local/austin/travis-county-esd-1-at-least-1-house-1-vehicle-on-fire-in-west-travis-county/


----

stellantis includes Fiat, Peugeot, Open, Citroen, Dodge

Stellantis is also Chrysler (it was a recent merger between Fiat/ Chrystler & Peugeot

Other potential partnership we have ex: Kinova - https://www.therobotreport.com/kinova-raises-48m-for-robotic-arms/

----



-----
useSWR

Stale While Revalidate: keep reference to the last value that is returned whenever 

take an array, create a local cache in the browser.


---
Call with Matheus regarding Studio Source

lookup is a function, not variable

lookup source / raw source


Tasks:
[bug] - When creating a new automation for the FF, it gets stuck on 'loading automation details'. Reloading the page worked. Tried again and same thing.
- Create the Raw source creation flow
- Update the sourceListRenderer to support the Raw Source
- Update the trigger logic to support selecting a RAW source as a variable options.
- Sort the list of operations by alphanumeric
- Modify the lookup based source components to not allow multi-device source type (in the short term only)
- Show some form of distinction bw the lookup source types and the raw source types

My short term tasks:
- P1: Modify the lookup based source components to not allow multi-device source type (in the short term only): the source type in the source section: disable the option.
- Create the Raw source creation flow.
	- new component to allow user to create a raw source on the automation.

----
## Studio Notes
[automationId].tsx

AutomationSectionListRender.tsx

StudioContext -> activeSection

Not just render the lookup_bindings, but also raw

AutomationStudioPanelRenderer.tsx
Two panels: 
- preview panel
- logic panel (empty - now we don't know what to put there)
decided by isConfiguringLogic (StudioContext.tsx)

isCreatingItem from StudioContext.tsx, drive rendering existing sources or show the creation card.

AutomationItemCreator.tsx
depends on if it's sources or conditions or

renderActiveCreationContent
case 'source'


console.log Latest Active Automation 
[conditions]
- expression: name of the function, parameter passed into the function
- id
- name

expression function is defined in [lookup_bindings] (is the source in frontend)
[lookup_bindings]
- id
- name
update of name doesn't change the id

[meta_data]
we don't put into this properties, we read this properties and it helps us to display some related info on the Manage project details
- devices
- types
- zones
We can use it as a filter tool

[sources]
This is the raw source array
array of object
- ofl_fleet_name
- path

[tasks]
Tasks can be triggered on fire or on reset.
AE automation engine check the trigger logic once per second.
second 1-3 condition is false
second 4 is true.
stay true for a few seconds until second 9.
second 9 is false.
fire at second 4
falling_edge at second 9
tasks.fire contains all the tasks should be done.
tasks.falling_edge contains all the tasks that should be clear.


[trace_enable]
no idea, ignore it

[trigger_logic]
TriggerLogicSection.tsx
Take in expression and render the Configure Logic Tab




----
Task
Create the Raw source creation flow.

AutomationSourceListRenderer.tsx
Rename SourceListItem to SourceLookupListItem (or not)
RawSourceListItem


automation-models.ts
type FreedomAutomation

type AutomationRawSource
- `device_id`: string 
	- "*" if users select multiple devices
- we gonna ignore `devices`
- `id`
- `name`
- `ofl_fleet_name`
- `path` 

If users select multiple devices, we want to have a input for the user to enter the fleet_name (dropdown - we need to build this dropdown).
Need to populate a new property in the studioContext: ofl_fleet_name: string[]

we have filteredOFLDevices in SourceListItem.tsx
we can look at the `ofl_fleet_name` property and get a list of available `ofl_fleet_name` for the dropdown options.ﬁ
const OFLFleetOptions = filteredOFLDevices.map(() => {})

![[Pasted image 20220215173851.png]]

 AutomationItemCreator.tsx




-----
## My notes
```ts
export type FreedomAutomationLookupBinding = {
  name: string;
  lookup_id: string;
  id: string;
  device_id?: string;
  zone_id?: string;
  ofl_type?: string;
};

export type AutomationRawSource = {
  device_id: string; // If the user chooses multiple devices then this string is `*` otherwise its the chosen devices devidId
  id: string; // This must be the name that was provided, with the white space stripped out, and append __id to the end of the name
  name: string; // Human readable name
  ofl_fleet_name: string;
  path: string[];
};

export type FreedomAutomation = {
  name: string;
  id: string;
  description?: string;
  project_id: string;
  enabled: boolean;
  error?: string;
  min_activation_time?: number; // this number represents seconds
  min_deactivation_time?: number; // this number represents seconds
  meta_data?: FreedomAutomationMetaData;
  lookup_bindings?: FreedomAutomationLookupBinding[];
  conditions?: FreedomAutomationCondition[];
  sources?: AutomationRawSource[];
  trigger_logic: string[] | Array<string[]>; // TODO -> This should be a LISP valid string array specifically.
  tasks?: AutomationTasksConfig;
};
```



```jsx
case 'list':
        return (
          <FormControl label={dynamicFormElement.label} vertical>
            <div style={{background: theme.palette.background}}>
              <JsonEditor
                name={`${dynamicFormElement.label}-dynamic-param-input`}
                initialValue={dynamicFormElement.value}
                value={dynamicFormElement.value}
                onChange={(value) => onChange(dynamicFormElement, value)}
                height="10rem"
              />
            </div>
          </FormControl>
        );
```


----

AutomationStudioHeader.tsx
get `activeAutomation` object from `StudioContext`
`activeAutomation.name` is the automation's name such as 'Multidevices'

return
`PageCommander` componnet: show automation name, settings button and error button.
if `activeAutomation.error` exist, show the error button

If `isConfiguringLogic` , render the `ActiveSectionContainer` with 
'Configure Trigger Logic' [tab title]
if not, render  `ActiveSectionContainer` with `{activeSection + 's'}`
string as [tab title]

`setIsCreatingItem` method comes from `StudioContext`
I need to support it with raw source option.
I want to add the raw source option in the side panel + button
`activeSection` need to support raw source option as well.
`activeSection` has type of [AutomationStudioSection], type include
- "source"
- "condition"
- "logic"
- "task"
- I need to add one more: "rawsource"


AutomationPreviewPanel.tsx





AutomationSectionListRenderer.tsx
return

isConfiguringLogic
if yes, render `TriggerLogicSection` component
else render `ListContainer` component

Inside `ListContainer`
we run the `renderActiveSection` to generate component depends on different `activeSection`
There are 3 types of `activeSection`:
- 'source'
- 'condition'
- 'task'



----
http://localhost:3000/account/AA2E4916C77670BAA7DA84BD4/automation/studio/AUTADCA02B84D6575FFED6BE5F4

https://app.freedomrobotics.com/account/AA2E4916C77670BAA7DA84BD4/device/D3C18A79A5DCC55E40DB6C76795/pilot


Update project variable
variables are provided by studio context.
Need to be able to save the project variable when it's changed.

Two new sections in the Configure Trigger Logic: 
- any variable of the projects
- raw sources (not lookup source, only the raw sources)








git stash






















