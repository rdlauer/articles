# Svelte 101

https://www.youtube.com/watch?v=JWVRs58rCKQ&list=PLyNmzPg2z6OiXylVCmsUOpaXdZm7lrzCt

svelte is a compiler - instead of shipping our code + a 3rd party library, transformed ("compiled") into highly optimized pure js
bundle size super small
must go through a build process

reactivity is key
think about excel spreadsheets and formulas - value of one cell depends on value of other cells
user interactions (or events) will trigger chain of internal state changes, in turn auto-updating ui elements

svelte apps built with components (not web components). allows code to be modular and easier to maintain. app ui is a component tree.

component defines how our app should interpret the abstract state values and translate them into meaningful ui elements
each .svelte file == a single component w/ three sections: script, style (scoped to current component), svelte template - defines how component should be structured

svelte template based on html of course. can also use custom components and inline logic w/ javascript (similar to jsx). first letter of custom component must be upper case.
to insert dynamic content, use {} to denote code inside is a javascript expression which needs to be evaluated
js expressions are usually seen on the right-hand side of a statement. like blah = ok ? 'yes' : 'no'
instead svelte runs expression then assigns the evaluated result to the spot where the curly braces are!

passing data via props (properties) to svelte components. strings or bools in child component. "export let color = blue"...to accept a prop, need "export" to expose a variable
setting props on parent (consuming) is same as setting a property on an html element whatever color="blue" or whatever color={color}. use curly braces to pipe dynamic data into child components
child components re-render when props change

user interactions trigger events. in svelte, use on:eventname to listen to events fired on that component. perform reaction to the event by assigning event handler function. for example on:input={handleInput} then in script tag function handleInput(e) {
https://www.youtube.com/watch?v=kdyJh0H4nxA&list=PLyNmzPg2z6OiXylVCmsUOpaXdZm7lrzCt&index=7
can also define inline (see vid)
two-way data binding with bind:value={text}

reactive statements - "$:" before any normal javascript statement in the script section.
svelte compiler will analyze which variables the statement depends on. when any dependent variables change, the statement will re-run..."reacting" to the updates.

sharing state with reactive stores, share state across multiple components that don't have parent/child relationship. 
reactive store exists in separate js file that is then imported into svelte components.
import stores like a regular component, but add "$" prefix to the variable names used, in order to use them like regular state variables

conditional rendering in ui:
{#if isAwesome}
asdf
{:else}
asdfasdf
{/if}
loop through and render list of elements {#each list as item, i} (i == current index)...end with {/each}
item looks like li {list[0]}...or li {item} with each syntax

async/await pattern for requests...like handling api requests
use await to wait for promises to resolve (like fetch) directly inside svelte template
template has an await block on the promise...see video
starts with placeholder content while promise is still in pending state
when promise is successfully resolved, use the :then section to define what to show
use :catch section to define what to display in case of any errors

svelte ships with a set of useful animated transitions
add transition:fade property to component
also add params to transition property like duration (see video)
first layer of { is to denote that it's a javascript expression
second layer is a javascript object
fade, blur, fly, slide (among others)
can also prepend in: out: as a shortcut for transitions

local setup, need latest node and npm
also need degit (useful)
npm degit sveltejs/template projectname
cd to project directory
npm i to install all dependencies
code .
npm run dev to spin up dev server on port 5000
npm run build for optimized production bundle of app (ends up in "public" directory)
