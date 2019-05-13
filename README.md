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
action name: NOUN_VERB; action creator name: verb + Noun; selector name: get + Noun
  
  
4. Request of data required to render some UI components has to be combined with special service component RenderOnReady, that can shows waiting status of request (e.g., spinner). RenderOnReady can wrap components required data from server and shows them only after required data will be received from server. Also RenderOnReady can contain component to handle possible errors on response.
```
const RenderOnReady = (props: Props) => {
  return (
    <>
      {props.errorObject ? (
          <ErrorModal errorObject={props.errorObject}/>
        )
        : (props.isReadyToRender || props.spinnerIsHidden) ? (
            props.children
          ) : (
           <Spinner/>
        )}
    </>
  );
};

class SampleComponent extends React.Component {
  constructor(props: Props) {
    super(props);
    this.state = {
      isReadyToRender: false,
      errorObject: null,
      isHideComponentTillGetData: true
    };
  }
  componentDidMount(){
    api.getSomeData()
      .then(response => {
        this.props.setSomeDataToStore(response.data);
        this.setState({isReadyToRender: true});
      })
      .catch(error => {
          this.setState({
            isReadyToRender: true,
            errorObject: apiErrorHandler(error)
          });
        });    
    }
  }
  render(){
    return (
    this.state.isHideComponentTillGetData ? 
      (<RenderOnReady isReadyToRender={this.state.isReadyToRender} errorObject={this.state.errorObject}>
        <SomeComponent/>
      </RenderOnReady>)
      :
      (<>
        <RenderOnReady isReadyToRender={this.state.isReadyToRender} errorObject={this.state.errorObject}/>
        <SomeComponent/>
      </>)
    );
  }
}
const mapDispatchToProps = dispatch => {
  return {
    setSomeDataToStore: (someData) => dispatch({
      type: actions.SOME_DATA_SET,
      value: someData})
  };
};
export default connect(null, mapDispatchToProps)(SampleComponent);
```

5. Apply wherever it possible principle 'the server is single source of trust'. If some data were updated by user input, they have to be fetched from server.

6. Do not rerender all layout for each route. Rerender only that part, which needed to change.





