Recoil types
=========================

To a second type parameter to selector of Recoil.


## Installation

To use Recoil Types with your app, install it as a dependency:

```bash
# If you use npm:
npm install recoil-types

# Or if you use Yarn:
yarn add recoil-types
```

## Example

Create a `userStore.ts` store file.

```ts
import { mapReducer, MapActions } from 'recruit';
import { atom, selector, DefaultValue } from 'recoil';

export interface User {
  id: number;
  name: string;
}

export type UserState = User | null;

export type UserActions = MapActions<UserState>;

export const initialState: UserState = null;

export const $userState = atom<UserState>({
  key: '$userState',
  default: initialState,
});

export const userState = selector<UserState, UserActions>({
  key: 'userState',
  get: ({get}) => get($userState),
  set: ({get, set, reset}, action) => {
    if (action instanceof DefaultValue) {
      return reset($userState);
    }

    return set($userState, mapReducer(get($userState), action));
  },
});
```

Create a `MyComponent.tsx` store file.

```tsx
import { useRecoilState } from 'recoil';
import { userState } from './userStore';

export default function MyComponents() {
  const [user, dispatch] = useRecoilState(userState);

  const set = () => {
    dispatch({
      type: 'set',
      payload: {id: 1, name: 'recoil'}
    });
  };
  const merge = () => {
    dispatch({
      type: 'merge',
      payload: {name: 'sanonz'}
    });
  };

  return (
    <div>
      <p>{JSON.stringify(user)}</p>
      <button onClick={set}>Set</button> | <button onClick={merge}>Merge</button>
    </div>
  );
}
```

## Issues

If you find a bug, please file an issue on [our issue tracker on GitHub](https://github.com/sanonz/react-recast/issues).

## License

[MIT](LICENSE.md)