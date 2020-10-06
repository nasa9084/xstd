# xstd

A Go code generator for extending standard packages

## Usage

Make a directory which has name same with standard package you want to extend and generate a file using xstd:

``` shell
$ mkdir context
$ cd context
$ xstd -pkg context > std_gen.go
```

Then start to extend the package and use it. You can just replace your import spec of the standard package to the package generated above.

### `excludes` option

If you want to redefine your extended version of constant/variable/function/type defined in standard package, you can use `-excludes` option.
The option takes a list of constant/variable/function/type names separated by comma, and excludes these from generated file so you can define
your own version but which has same name with standard package's one.

## Example

Here is an example with `context` package.

``` shell
$ mkdir context
$ cd context
$ xstd -pkg context > std_gen.go
```

Then create a source code file with content below:

``` go
package context

type requestIDKey struct{}

// WithRequestID returns a copy of given context in which associated with the request ID.
func WithRequestID(parent Context, requestID string) Context {
	return WithValue(parent, requestIDKey{}, requestID)
}

// RequestID returns the request ID associated with the context, or empty string if no
// request ID is associated with the context using WithRequestID.
func RequestID(ctx context.Context) string {
	if requestID, ok := ctx.Value(requestIDKey{}).(string); ok {
		return requestID
	}

	return ""
}
```

Now you can use this local `context` package with same way of standard `context` package, but local one has two extra functions named `WithRequestID` and `RequestID`.
