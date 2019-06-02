Summary of the [Java I/O tutorial](https://docs.oracle.com/javase/tutorial/essential/io/index.html).

# Java I/O Streams

## Byte streams

* InputStream (implements Closeable)
	* ByteArrayInputStream
	* **FileInputStream**
	* FilterInputStream
		* **BufferedInputStream**
		* DataInputStream (implements DataInput)
		* LineNumberInputStream
		* PushbackInputStream
	* ObjectInputStream (implements ObjectInput, ObjectStreamConstants)
	* PipedInputStream
	* SequenceInputStream
	* StringBufferInputStream
* OutputStream (implements Closeable, Flushable)
	* ByteArrayOutputStream
	* **FileOutputStream**
	* FilterOutputStream
		* **BufferedOutputStream**
		* DataOutputStream (implements DataOutput)
		* PrintStream (implements java.lang.Appendable, Closeable)
	* ObjectOutputStream (implements ObjectOutput, ObjectStreamConstants)
	* PipedOutputStream

All byte stream classes are descended from the abstract `InputStream` and `OutputStream` classes. All other stream types are built on byte streams.

**Use**: a `FileOutputStream` wrapped in a `BufferedOutputStream`. Same for an input stream.

## Character streams

* Reader (implements Closeable, java.lang.Readable)
	* **BufferedReader**
		* LineNumberReader
	* CharArrayReader
	* FilterReader
		* PushbackReader
	* **InputStreamReader**
		* FileReader
	* PipedReader
	* StringReader
* Writer (implements java.lang.Appendable, Closeable, Flushable)
	* **BufferedWriter**
	* CharArrayWriter
	* FilterWriter
	* **OutputStreamWriter**
		* FileWriter
	* PipedWriter
	* PrintWriter
	* StringWriter

All character stream classes are descended from the abstract `Reader` and `Writer`.

On `FileReader` vs `FileInputStream`:

> Notice that both `CopyBytes` and `CopyCharacters` use an `int` variable to read to and write from. However, in `CopyCharacters`, the `int` variable holds a character value in its last **16 bits**; in `CopyBytes`, the `int` variable holds a byte value in its last **8 bits**.

Character streams are often "wrappers" for byte streams. The character stream uses the byte stream to perform the physical I/O, while the character stream handles translation between characters and bytes. `FileReader`, for example, uses `FileInputStream`, while `FileWriter` uses `FileOutputStream`.

There are two general-purpose byte-to-character "bridge" streams: `InputStreamReader` and `OutputStreamWriter`. Use them to create character streams when there are no prepackaged character stream classes that meet your needs.

**Use**: an `OutputStreamWriter` wrapped in a `BufferedWriter`. Same for a input stream. Avoid `FileReader` and `FileWriter` because you cannot specify the chartset.

## Buffered streams

There are four buffered stream classes used to wrap unbuffered streams: `BufferedInputStream` and `BufferedOutputStream` create buffered byte streams, while `BufferedReader` and `BufferedWriter` create buffered character streams.A buffered stream might require to be flushed.

## Data streams

* DataInput
	* ObjectInput (also extends java.lang.AutoCloseable)
* DataOutput
	* ObjectOutput (also extends java.lang.AutoCloseable)

Data streams support binary I/O of primitive data type values (`boolean`, `char`, `byte`, `short`, `int`, `long`, `float`, and `double`) as well as `String` values.

All data streams implement either the `DataInput` interface or the `DataOutput` interface. The most widely-used implementations of these interfaces are `DataInputStream` and `DataOutputStream`.

## Object streams

Just as data streams support I/O of primitive data types, object streams support I/O of objects. The object stream classes are `ObjectInputStream` and `ObjectOutputStream`. These classes implement `ObjectInput` and `ObjectOutput`, which are subinterfaces of `DataInput` and `DataOutput`.
