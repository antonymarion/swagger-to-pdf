# swagger-to-pdf

### Usage:

    usage: swagger2pdf [-h] [-v] [-s SRC] [-j] [-y] [-o OUTPUT]

    Optional arguments:
      -h, --help            Show this help message and exit.
      -v, --version         Show program's version number and exit.
      -s SRC, --src SRC     Swagger spec JSON file.
      -j, --json            Spec file in JSON format (default).
      -y, --yaml            Spec file in YAML format.
      -o OUTPUT, --output OUTPUT
                            Output path of the pdf file.

### Exec:

For JSON files:

swagger2pdf -j -s ./swagger-spec-sample.json -o ./dump/

For YAML files:

swagger2pdf -y -s ./swagger-spec-sample.yaml -o ./dump/

In either case, the execution will take several seconds to complete.

### Sample:
See: ./dump/swagger-spec-sample.pdf

### Change log:
#### 1.1.0: Support swagger spec files in YAML format, see: [PR#13](https://github.com/antonymarion/swagger-to-pdf/pull/13/files).

### Appendix:
#### Working Theory

In the case of a JSON input file:

1. Read source swagger json spec
2. Convert it into yaml format
3. Dump yaml into the spec dir of swagger editor
4. Use electron to render swagger editor page (nicely formatted api page)
5. Dump the page into pdf

In the case of a YAML input file, steps 1-2 are bypassed.

#### Swagger Editor
Version of swagger editor embedded is: 2.9.9   
You can get it from: [here](https://github.com/swagger-api/swagger-editor)   
If you want to use some other version:

* clone the repo
* replace the editor in ./swagger-editor
* Add codes below into the bottom of file ***swagger-editor/index.html***
* npm install .

```javascript
<script type="text/javascript">
    $("document").ready(function() {
        setTimeout(function() {
            var fileMenuTimer = setInterval(function() {
                // open file menu
                var fileMenu = $("#fileMenu");
                if (fileMenu.length) {
                    clearInterval(fileMenuTimer);
                    fileMenu.click();

                    // open example selector
                    var exampleButton = $("#open-example");
                    var exampleButtonTimer = setInterval(function() {
                        if (exampleButton.length) {
                            clearInterval(exampleButtonTimer);
                            exampleButton.click();

                            // click open button
                            setTimeout(function() {
                                $(".modal-large .btn-primary").click();
                            }, 1000);
                        }
                    }, 300);
                }
            }, 300);
        }, 1000);
    });
</script>
```

Done, just use it.
