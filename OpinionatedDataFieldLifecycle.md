### Opinionated Data Field Lifecycle States

One annoyance for users when there is a lot of data entry is the lack of immediate 
feedback for invalid data entry. Adding a server round-trip to the mix 
compounds this annoyance and is a waste of network bandwidth. 

To solve this problem a set of front-end component lifecycle metadata states 
can be attached to Front-End components. These components can then be styled 
as needed to visually display the lifecycle state to the user at a glance.
For example, if a text field accepts only numbers but a user enters letters,
the background of the field could automatically turn red when the first letter 
is typed. To further simplify this process, keystroke and mouse events could be 
tied directly to an automatically generated [JSON Schema](Validation.md).

There are several possible situations where a component could be styled to
indicate to the user the state of a component:

 - [Asynchronously](https://www.w3schools.com/js//js_asynchronous.asp) loading data 
   using a library such as [Axios Http](https://axios-http.com/) leaves Front-End 
   Components out-of-sync with the underlying data state which is "loading data".
   When these components wait to receive data from a server, the old data in a component 
   is no longer valid yet the new data is not yet available. A user should be able to 
   continue interacting with other parts of the application while data is loading. 
   For example, perhaps the user made a 
   mistake and wants to load different data instead. Locking the entire application is 
   a nuisance. Instead, individual data fields could be made inactive and grayed so that 
   the user knows something is happening. Flashing ellipses or some other indicator 
   could be styled into the component to indicate that a component is loading. 
   [Progressive Web Apps](https://web.dev/progressive-web-apps/) solve part of this problem 
   but not at a field level.

 - Data which has been unvisited and is invalid is different from data which was visited
   and invalid. When creating new data the screen should not be cluttered with additional
   styling such as for example creating red borders where data is required but not entered.

 - Loaded data which has not been modified should be easily identified on a screen.
   This allows a user to scan a screen quickly and ignore the unmodified data.

 - Data which has been modified and has passed field validation should be displayed
   in a way so that they can be easily found. For example, the border of a field could
   be green.

 - Data which has been modified and is invalid should be display in a way, so
   it can be easily found. For example, the border of a field could be red.

 - When saving data, buttons and relevant fields being saved should be styled.

 - When there is invalid data, buttons which allow the data to be saved should be 
   invalidated and styled. [Lifecycle Metadata](Lifecycle-Metadata.md) can be scanned 
   to determine the state of the component.

Styles for a component should be separate from the lifecycle state of a component.
A field should not always be styled with a red border. These styles should be placed
in the styling section of an application.

Each component is tied to a piece of data which can be 
in one of several states: loading, saving, valid, required-not-supplied, invalid, etc.
Each state for each component then can be tied using [BEM](http://getbem.com/) to a unique set of styles which are
automatically attached as each component enters different states.
