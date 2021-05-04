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
- Query : DB로 부터 정보를 얻는것
- Mutation : 정보를 DB로 보내는것

```graphql
//리턴 타입을 설정할 수 있다.
type Movie {
   id :Int!
   ...
}

type Query {
//required => !를 붙인다.
 name: String!
 
 // Arguments가 필요할 경우 "쿼리명(파라미터:타입) : 리턴타입"으로 지정한다.
 movie(id:Int!) : Movie
 
 // 다수의 결과를 받아오려면 "쿼리명: [리턴타입]"으로 지정한다.
 movies : [Moive]!
}

type Mutation{
    ...
}

```
[schema.graphql]

## resolver
- schema에서 설명한 Query, Mutation를 Resolver에서 프로그래밍 한다.
- schema에서 지정한 이름과 다르면 에러를 발생한다.

```javascript
const resolvers = {
    Query: {
       ...
    },
    Mutation: {
        ...
    }
}
```
[resolvers.js]

## 예제
```javascript
import { GraphQLServer } from "graphql-yoga";
import resolvers from "./graphql/resolvers";

const server = new GraphQLServer({
  typeDefs: "graphql/schema.graphql",
  resolvers,
});

server.start(() => console.log("Graphql Server Running"));

```
[index.js]
```graphql
//리턴 타입을 설정할 수 있다.
type Movie {
  id: Int!
  title: String!
  rating: Float!
  summary: String!
  language: String
  medium_cover_image: String
  genres: [String]
  description_intro: String
}

type Query {
 name: String!
 movie(id:Int!) : Movie
 movies : [Moive]!
}

```
[schema.graphql]

```javascript
const resolvers = {
    Query: {
        name: (_,args) => "test",
        movie: async (_,{id}) => {
          const {
            data: {
              data: { movie },
            },
          } = await axios(MOVIE_DETAILS_URL, {
            params: {
              movie_id: id,
            },
          });
          return movie;
        },
        movies : () => {..}
        movieData : () => {} // error : schema에 정의되지 않음
    }
}
```
[resolvers.js]

