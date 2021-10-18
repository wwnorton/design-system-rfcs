

# RFC : Technical design document

## Component - Tab

### Scope of the Document
This document outlines the technical design document of Tab component functionalities. Its highlights the component design, properties, and initial implementation.

### Component Overview
Tabs organize and allow users to move between content without having to navigate away from a page. The Tabs component organizes multiple sections of related content so a user can view one section at a time.

### Component Design
> Component designed based on the scope of the tab component, by default, the Tab simply renders data passed to it.  
 ### Scope  
 - Tab Container: Tab container is a component that manages the context and state of the tabs.   
 - Tab List: Wrapper for the tab components.  
 - Tab:  Element that serves as a label for one of the tab panels and can be activated to display that panel.  
 - Tab Panel: Is a presentational component which accepts a tab’s content as its children.  

 #### Tab Container  
The component or element the *Tabs* should render as *Fragment*. 

Provides context and state.
##### Properties
| Props  | Type | Default | Required | Description |
| :------------- |:-------------:|:-------------:|:-------------:|:-------------|
| ClassName| Object | | | Override or extend component style.|
| Id      | String | | | Unique identification of tabs. |

#### TabList
The component or element the *TabList* should render as *div*.
##### Properties
| Props  | Type | Default | Required | Description |
| :------------- |:-------------:|:-------------:|:-------------:|:-------------|
| ClassName| Object | | | Override or extend component style.|
| Id      | String | | | Unique identification of tabs. |
| aria-label      | String | | | Defines a string value that labels the current element. |
| Orientation      | Variant | Horizontal| | Tab should display Horizontal or Vertical. |
| aria-orientation      | Variant | =Orientation| | Indicates whether the element's orientation is horizontal, vertical. |
| SelectedIndex      | Number | 0| | Default selected Index. |
| onSelected      |  | (index: number) => void| | Default selected Index. |

#### Tab

The component or element the ‘**_Tab’_** should render as ‘**_button’._**
Children of Tablist component, display based on label, by default index is 0
##### Properties
| Props  | Type | Default | Required | Description |
| :------------- |:-------------:|:-------------:|:-------------:|:-------------|
| Active| Boolean | | | Selected, Unselected flag, based on this flag active tab.|
| Disabled      | Boolean | | | <![endif]--> Disabled tab, should not be selected. |
| defauItIndex      | Number | 0| | This allows changing the tab that should be open on initial render. This is a zero-based index. _This can only be used in uncontrolled mode when tabs handle the current selected tab internally and for this reason cannot be used together with selectedIndex._ |
| Id      | String | | | Unique identification of tablist. |
| ClassName      | Object | | =Orientation| Override or extend component style. |

#### TabPanel
The component or element the ‘**_TabPanel’_** should render as ‘**_div’._**

element that contains the content associated with a tab
##### Properties
| Props  | Type | Default | Required | Description |
| :------------- |:-------------:|:-------------:|:-------------:|:-------------|
| ClassName| Object | | | Override or extend component style.|
| Id      | String | | | Unique identification of tabs. |
| Children      | Node | | | Override or extend component style. |

 ### Design Flow
 <img width="578" alt="image" src="https://user-images.githubusercontent.com/25932161/137789481-c6078525-413c-47b8-b520-498934b99f6b.png">


### Component Style
#### Mixings

 - nds—tabs (base)
 - nds-tabs__tablist
 - nds-tabs__tabpanel
 - nds—tabs__tab

### Keyboard Intraction

| Key  | Action|
| :------------- |:-------------|
| ArrowLeft| Move focus to previous column. | 
| ArrowRight| Move focus to next column. | 
|Tab	| Focus moves into the tablist from the place of active tab.
|Space or Enter	| Activate tab if tabs are not activated.
|Home	| Focus move to first tab.
|End	| Focus moves to last tab.

## Accessibility

### ARIA Roles
#### Tab

| Role  | Description|
| :------------- |:-------------|
| role=”tab”| Indicate that is tab. | 
| aria-selected| Set for active tab. | 
|aria-control	| Set associate idb.

#### TabList

| Role  | Description|
| :------------- |:-------------|
| role=”tablist”| Indicate that is tablist. | 
| aria-orientation| Indicate horizontally or vertically tab. | 

## Implementation example 


    <Tabs index={tabIndex} onSelect={handleTabsChange}>
	    <TabList>
		    <Tab>One</Tab>
		    <Tab>Two</Tab>
		    <Tab>Three</Tab>
	    </TabList>
	    <TabPanel>	
		    <span>This is tab one</span>
	   </TabPanel>
	   <TabPanel>
		   <p>This is tab two</p>
	   </TabPanel>
	   <TabPanel>
		   <div>This is tab three. </div>
	   </TabPanel>
	</Tabs>

## Component render example

    <Fragment>
            <div role="tablist >
              <button id={$TAB_ID} aria-controls={$PANEL_ID} aria-selected={boolean} >One</ button >
              <button id={$TAB_ID} aria-controls={$PANEL_ID} aria-selected={boolean} >Two</ button >
              <button id={$TAB_ID} aria-controls={$PANEL_ID} aria-selected={boolean} >Three</ button >
            </div>
             <div role="tabpanel" id={$PANEL_ID} aria-labelledby={$TAB_ID}>
                <span>This is tab one</span>
              </div>
              <div role="tabpanel" id={$PANEL_ID} aria-labelledby={$TAB_ID}>
                <p>This is tab two</p>
              </div>
              <div role="tabpanel" id={$PANEL_ID} aria-labelledby={$TAB_ID}>
                <div>This is tab three. </div>
              </div>
    </ Fragment >

### Anatomy document
[https://docs.google.com/document/d/1D0MjvCYdCaaHDJGYcPk8u9pSREkVpAac8JqNYzviHc0/edit#heading=h.5old1136lvpw](https://docs.google.com/document/d/1D0MjvCYdCaaHDJGYcPk8u9pSREkVpAac8JqNYzviHc0/edit#heading=h.5old1136lvpw)

### Refs
https://www.w3.org/TR/wai-aria-practices-1.1/examples/tabs/tabs-1/tabs.html


