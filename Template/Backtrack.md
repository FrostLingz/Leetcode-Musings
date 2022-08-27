# Backtrack

```Java
result = []
def backtrack(path, selection list):
    //base case
    if (Meet the end condition) {
        result.add(path);
        return;
    }

    for (selection in selection list) {
        //make a selection
        Remove this selection from the selection list
        path.add(selection);
        backtrack(selection, selection list);
        //withdraw selection
        path.remove(selection);
        Add the selection back to the selection list
    }
  ```
