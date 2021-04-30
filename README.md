## 프로젝트 목적(movieql)
Graphql 기본 개념 공부 및 graphql-yoga를 이용해 간단한 영화 api 만들기

## 설치
graphql-yoga 설치
```
# npm 
npm install graphql-yoga

# yarn
yarn add graphql-yoga
```
nodemon 설치 : 파일을 수정할때 마다 서버를 재시작 해줌
```
# npm 
npm install nodemon

# yarn
yarn add nodemon
```

babel 설치
```
# npm 
npm install babel-node babel-cli babel-preset-env babel-preset-stage-3

# yarn
yarn add babel-node babel-cli babel-preset-env babel-preset-stage-3
```

## 설정 
```
{
...
"scripts": {
    "start": "nodemon --exec babel-node index.js"
  },

}
```
[pakage.json]
```
{
    "presets": ["env","stage-3"]
}
```
[.babelrc]


## graphql로 해결할수 있는 두가지 문제 
1. Over-fetching : 요청한 영역의 정보보다 많은 정보를 서버에서 받아오는 것.
2. Under-fetching : REST에서 하나를 완성하려고 많은 소스를 요청하는 것. 


## schema
사용자에게 보내거나 사용자로부터 받을 data에 대한 설명
Query : DB로 부터 정보를 얻는것
Mutation : 정보를 DB로 보내는것

```
type Query {
 name: String!
}


```

## 예제
```
import { GraphQLServer } from "graphql-yoga";
import resolvers from "./graphql/resolvers";

const server = new GraphQLServer({
  typeDefs: "graphql/schema.graphql",
  resolvers,
});

server.start(() => console.log("Graphql Server Running"));

```

