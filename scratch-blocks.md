## scratch的自定义block

### Blockly 创建自定义 blocks

用 js API来创建：

```js
var mathChangeJson = {
  "message0": "change %1 by %2",
  "args0": [
    {"type": "field_variable", "name": "VAR", "variable": "item", "variableTypes": [""]},
    {"type": "input_value", "name": "DELTA", "check": "Number"}
  ],
  "previousStatement": null,
  "nextStatement": null,
  "colour": 230
};

Blockly.Blocks['math_change'] = {
  init: function() {
    this.jsonInit(mathChangeJson);
    // Assign 'this' to a variable for use in the tooltip closure below.
    var thisBlock = this;
    this.setTooltip(function() {
      return 'Add a number to variable "%1".'.replace('%1',
          thisBlock.getFieldValue('VAR'));
    });
  }
}
```

#### Block colour

```json
{
  "color": 160
}
```

```js
this.setColour(160);
```

#### Statement Connections

```json
{
  "nextStatement": null,         // untyped
  "nextStatement": "Action",     // typed
  "previousStatement": null      // untyped
  "previousStatement": "Action", // typed
}

```

```js
this.setPreviousStatement(true, 'Action');
```

#### Block Output

```json
{
  "output": null,
  "output": "Number"
}

```

```js
this.setOutput(true, 'Number');
```

#### Block Inputs

Fields 类型：

- field_input
- field_dropdown
- field_checkbox
- field_colour
- field_number
- field_angle
- field_variable
- field_date
- field_label
- field_image

Inputs 类型：

- input_value
- input_statement
- input_dummy

```js
this.appendDummyInput()
this.appendField('string')
this.appendField(new Blockly.FieldLabel('Neil', 'person'))
this.appendField(new Blockly.FieldImage("https://demo.gif", 15, 15, "*"))
this.appendField(new Blockly.FieldTextInput('default text'),'FIELDNAME')
this.appendField(new Blockly.FieldDropdown([
  ['first item', 'ITEM1'],
  ['second item', 'ITEM2']
]), 'FIELDNAME')
this.appendField(new Blockly.FieldCheckbox('TRUE'), 'FIELDNAME')
this.appendField(new Blockly.FieldColour('#ff0000'), 'FIELDNAME')
this.appendField(new Blockly.FieldVariable('x'), 'FIELDNAME')
this.appendField(new Blockly.FieldNumber('100'), 'FIELDNAME')
this.appendField(new Blockly.FieldAngle(90), 'FIELDNAME')
this.appendField(new Blockly.FieldDate('2015-02-05'), 'FIELDNAME')


this.appendValueInput('LIST')
this.appendStatementInput('stmt')
this.setCheck('Array')
this,setAlign(Blockly.ALIGN_RIGHT) // Blockly.ALIGN_LEFT

// inline vs external
this.setInputsInline(true)

this.setTooltip('Tooltip text')

// change listeners
this.setOnChange(function(changeEvent) {
  if (this.getInput('NUM').connection.targetBlock()) {
    this.setWarningText(null);
  } else {
    this.setWarningText('Must have an input block.');
  }
})
```

#### 前置 block 配置

```js
block.setDeletable(false)
block.dispose()
block.setEditable(false)
block.setMovable(false
```

#### 扩展 block

```json
{
  "extensions": ["break_warning_extension", "parent_tooltip_extension"],
}
```

```js
Blockly.Extensions.register('parent_tooltip_extension',
  new function() {
    // this refers to the block that the extension is being run on
    this.setTooltip(function() {
      var parent = thisBlock.getParent();
      return (parent && parent.getInputsInline() && parent.tooltip) ||
        Blockly.Msg.MATH_NUMBER_TOOLTIP;
    });
  });

```


### scratch-block 打包过程 build.py

复制 node_modules/google-closure-library 到上级目录:

```sh
cp -r node_modules/google-closure-library ../closure-library
```

然后依次执行:

```python
search_paths_vertical = "core/block_render_svg_vertical.js"
search_paths_horizontal = "core/block_render_svg_horizontal.js"
Gen_uncompressed(search_paths_vertical)
Gen_uncompressed(search_paths_horizontal)
Gen_compressed(search_paths_vertical, search_paths_horizontal)
Gen_langfiles()
```

webpack 打包了所有模块：

dist/web 下为 Blockly
dist 下为 ScratchBlocks

#### 引入顺序

压缩版本：

```js
<script src="blockly_compressed.js"></script>
<script src="blocks_compressed.js"></script>
<script src="javascript_compressed.js"></script>
<script src="msg/js/en.js"></script>
```

非压缩版本：

```js
<script src="blockly_uncompressed.js"></script>

<script src="blocks/logic.js"></script>
<script src="blocks/loops.js"></script>
<script src="blocks/math.js"></script>
<script src="blocks/text.js"></script>
<script src="blocks/lists.js"></script>
<script src="blocks/colour.js"></script>
<script src="blocks/variables.js"></script>
<script src="blocks/procedures.js"></script>

<script src="generators/javascript.js"></script>
<script src="generators/javascript/logic.js"></script>
<script src="generators/javascript/loops.js"></script>
<script src="generators/javascript/math.js"></script>
<script src="generators/javascript/text.js"></script>
<script src="generators/javascript/lists.js"></script>
<script src="generators/javascript/colour.js"></script>
<script src="generators/javascript/variables.js"></script>
<script src="generators/javascript/procedures.js"></script>

<script src="msg/messages.js"></script>
```
