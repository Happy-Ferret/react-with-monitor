# react-with-monitor


### Installation

    npm i react-native-buttonex

### Usage

A `monitor` function is injected into the wrapped component. Access it at `this.monitor`. This function accepts a function, which gets two arguments, `props` and `state`. It returns a promise, which is resolved when the function returns truthy.

    import withMonitor from 'react-with-monitor'

    async function delay(ms) {
        await new Promise(resolve => setTimeout(()=>resolve(), ms));
    }

    class LoadingDumb extends Component {
        state = {
            step: 0
        }

        componentDidMount() {
            this.orchestrate();
        }

        render() {
            const { isPreLoaded } = this.props;

            return (
                <View style={styles.screen}>
                    { !isPreLoaded && <Image source={{ uri:'splash' }} style={{ width:144, height:144 }} /> }
                </View>
            )
        }

        async orchestrate() {

            this.setState({ step: 1 });
            const isStep1 = await this.monitor((props, state) => state.step === 1);

            this.setState({ step: 2 });
            await this.monitor((props, state) => state.step === 2);

        }
    }

    const Loading = withMonitor(LoadingDumb)

    export default Loading
