# AS Best Practices and Useful Notes

### Content
#### [Javascript](#heading1)
#### [React](#heading2)
#### [Redux](#heading3)
#### [Testing](#heading4)
#### [React Project on CRA](#heading5)

<a name="heading1"/>

## Javascript
There will be vanilla javascript notes.


<a name="heading2"/>

## React
There will be React notes.


<a name="heading3"/>

## Redux
There will be Redux notes.


<a name="heading4"/>

## Testing
There will be testing notes.


<a name="heading5"/>

## React Project on CRA

1. Use plop library to generate React dumb components and Redux reducers. It can generate folder with component, style and test files. Run command `npm run plop` and type data (part of folder path, name of component) on requested input. Templates for generating are in `/plop-templates`, generation settings are in `plopfile.js`.

2. Handle waiting for load image on hover problem with special component `HoveredIcon`. It renders two icons (main and hover) absolute positioning with hover icon above the main icon. Hover icon gets null opacity and appears visible on hover. So, both images are loaded and there is no flush at icon hover.

3. Naming convention for Redux actions (source - (https://medium.com/@kylpo/redux-best-practices-eef55a20cc72)):
  action name: <NOUN>_<VERB>
  action creator name: <verb><Noun>
  selector name: get<Noun>




