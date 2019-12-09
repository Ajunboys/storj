# Notes for repository split

## TODO

* Create a repository for all our testing, such that the jenkinsfile could be in sync or shared. Otherwise we need to manually keep staticcheck, go, golint etc. settings in sync.

## Potential Problems

### API breaking changes

We cannot make any backwards incompatible changes directly. This means we need to do these in several steps.

1. Add new API.
2. Update all other repositories to use new API.
3. Deprecate old API.

#### Example

Lets say we want to change from `Upload(name string, data []byte)` to `Upload(name string, r io.Reader)`.

PR 1. Add new API:

```
+ UploadStream(name string, r io.Reader)
```

PR 2\*. Change other repositories:

```
- Upload(name, data)
+ UploadStream(name, bytes.NewReader(data))
```

PR 3. Remove from original repository:

```
- UploadStream(name string, data []byte)
```
