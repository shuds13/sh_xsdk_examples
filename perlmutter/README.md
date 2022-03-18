Trying with gcc compiler.

perlmutter has 4 compilers - default is nvidia, but spack does not seem to find it.

1.  Clone from gitlab xsdk-examples repo - and xsdk-examples branch.

2. Make packages.yaml with as much module use as possible for non-xsdk packages.

3. Build xsdk 

4. Build xsdk-examples using ^xsdk

3 and 4 could be combined - but separating for simplicity.

        
Future steps:
- Run examples.
- Build with +cuda.
- Run examples with +cuda.


