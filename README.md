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
action name: NOUN_VERB
action creator name: verb + Noun
selector name: get + Noun
  
  
4. Request of data required to render some UI components has to be combined with special service component RenderOnReady, that can shows waiting status of request (e.g., spinner). RenderOnReady can wrap components required data from server and shows them only after required data will be received from server. Also RenderOnReady can contain component to handle possible errors on response.
```
class Contract extends React.Component <Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = {
      isReadyToRender: false,
      errorObject: null
    };
  }
  setContractMainData = (contractMainData) => {
    this.props.onGetContractMainData(contractMainData);
    this.setState({isReadyToRender: true});
  };
  componentDidMount(){
    const contractId = +this.props.match.params.id;
    const contractMainData = this.props.contracts.find(item => item.id === contractId);
    if (contractMainData) {
      //TODO: is weak place, params as nested objects can be connected searchData.contracts. Util fn cloneObject may be applied
      this.setContractMainData(contractMainData);
    } else {
      api.getContractById(contractId)
        .then(response => {
          this.setContractMainData(response.data);
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
    const title = this.props.contractId ? concatIdName(this.props.contractId, this.props.name) : "";
    return (
      <RenderOnReady isReadyToRender={this.state.isReadyToRender} errorObject={this.state.errorObject}>
        <div className="Contract">
          <FixedHeader>
            <HeaderContent
              titleId={contractViewPageIds.headingContractTitle}
              title={title}
              info={this.props.contractNo}/>
          </FixedHeader>
        </div>
      </RenderOnReady>
    );
  }
}
const mapStateToProps = state => {
  return {
    contracts: state.organization.searchData.contracts,
    contractId: state.organization.item.contractMainData.id,
    name: state.organization.item.contractMainData.name,
    contractNo: state.organization.item.contractMainData.contractNo
  };
};
const mapDispatchToProps = dispatch => {
  return {
    onGetContractMainData: (contractMainData) => dispatch({
      type: contractMainDataActions.CONTRACT_MAIN_DATA_SET,
      value: contractMainData})
  };
};
export default connect(mapStateToProps, mapDispatchToProps)(Contract);
```



