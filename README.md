# taro-apollo

仿照 [react-apollo](https://github.com/apollographql/react-apollo) 1.x版本 以及 [taro-redux](https://github.com/NervJS/taro/tree/master/packages/taro-redux)做的 graphql componet wrapper

# 安装
```
npm install taro-apollo --save
```
或者
```
yarn add taro-apollo
```

# 使用

初始化apollo client 
这里我使用的是我的[wx-apollo-fetcher](https://github.com/kdong007/wx-apollo-fetcher) 你可以使用自己的或者其他fetch polyfill
```js
import { ApolloClient } from "apollo-client";
import { InMemoryCache } from "apollo-cache-inmemory";
import { HttpLink } from "apollo-link-http";
import wxApolloFetcher from "wx-apollo-fetcher";
import { setApolloClient } from "taro-apollo";

const client = new ApolloClient({
    link: new HttpLink({
        uri: "xxx",
        fetch: wxApolloFetcher,
    }),
    cache: new InMemoryCache(),
});

setApolloClient(client);
```

**apollo组件化**
```js

import { withQuery } from "taro-apollo";
import gql from "graphql-tag";

const query = gql`
    query xxx{
        xxxx
    }
`;

@withQuery({
    query: query,
    variables: (props, state) => {
        return {
            // xxx
        };
    },
})
export default class MyComponent extends Taro.Component {

    render() {
        const { data, loading, error } = this.props;
        return (
            <View>
                {loading && <View>加载中</View>}
                {error && <View>出错啦</View>}
                {data && <View>xxxx</View>}
            </View>
        );
    }

}
```
有需要注意的是我把原有skip和variables逻辑二合一了 当query需要variables && variables结果为空时自动skip


**直发query/mutation**
```js
import { sendQuery, sendMutation } from "taro-apollo";

sendQuery(query, variables, ignoreCache)
    .then(data => {
        // do something
    })
    .catch(err => {
        // handle error
    });

sendMutation(mutation, variables, refetchQueries)
    .then(data => {
        // do something
    })
    .catch(err => {
        // handle error
    });
```

# TODO
- withMutation

