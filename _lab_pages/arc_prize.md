---
layout: post
title:  ARC Prize Testing Interface
date:   2024-8-16
categories: kbai
permalink: /kbai/arc-prize/
author: Christopher J. MacLellan (Interface modified from <a href="https://github.com/fchollet/ARC-AGI">ARC-AGI Github</a>)
comments: false
---

<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
<script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js"></script>
<script src="/assets/js/arc-common.js"></script>
<script src="/assets/js/arc-testing_interface.js"></script>

<link rel="stylesheet" type="text/css" href="/assets/css/arc-common.css">
<link rel="stylesheet" type="text/css" href="/assets/css/arc-testing_interface.css">

<link href="https://fonts.googleapis.com/css?family=Open+Sans&display=swap" rel="stylesheet">

<div id="workspace">

<div id="demonstration_examples_view">
<div class="text" id="task_demo_header">Task demonstration</div>
<div id="task_preview"></div>
</div>

<div id="evaluation_view">

<div id="evaluation-input-view">
<div class="text">Test input grid <span id="current_test_input_id_display">0</span>/<span id="total_test_input_count_display">0</span>
<button onclick="nextTestInput()">Next test input</button>
</div>

<div id="evaluation_input" class="selectable_grid"></div>
</div>

<div id="evaluation_output_editor">

<div id="load_task_control_btns">

<p>
<label for="load_task_file_input">Load custom task JSON: </label>
<input type="file" id="load_task_file_input" class="load_task" style="display: none;"/>
<input type="button" value="Browse..." onclick="document.getElementById('load_task_file_input').click();" />
</p>

<!---
<p>
<label for="task_nav_controls">Choose Publich ARC-AGI Set:</label>
<button onclick="selectTraining()" id="training_btn"> Training Set </button>
<button onclick="selectEvaluation()" id="evaluation_btn"> Evaluation Set </button>
</p>
--->

<p>
<label for="task_nav_controls">Navigate ARC-AGI Problems:</label>
<button onclick="previousTask()" id="previous_task_btn"> Previous </button>
<button onclick="nextTask()" id="next_task_btn"> Next </button>
<button onclick="randomTask()" id="random_task_btn"> Random </button>
</p>
<p>
<label id='task_name' for="random_task_btn"> Task name: </label>
</p>
<p>
<label for="show_symbol_numbers">Show symbol numbers: </label>
<input type="checkbox" id="show_symbol_numbers" name="show_symbol_numbers" onchange="changeSymbolVisibility()">
</p>
</div>

<div id="edition_view">
<div id="editor_grid_control_btns">
<div id="resize_control_btns">
<label for="output_grid_size">Change grid size: </label>
<input type="text" id="output_grid_size" class="grid_size_field" name="size" value="3x3">
<button onclick="resizeOutputGrid()" id="resize_btn">Resize</button>
</div>

<button onclick="copyFromInput()">Copy from input</button>
<button onclick="resetOutputGrid()">Reset grid</button>
<button onclick="submitSolution()" id="submit_solution_btn">Submit!</button>
</div>

<div id="output_grid">
<div class="edition_grid selectable_grid">
<div class="row">
<div class="cell" symbol="0" x="0" y="0"></div>
<div class="cell" symbol="0" x="0" y="1"></div>
<div class="cell" symbol="0" x="0" y="2"></div>
</div>
<div class="row">
<div class="cell" symbol="0" x="1" y="0"></div>
<div class="cell" symbol="0" x="1" y="1"></div>
<div class="cell" symbol="0" x="1" y="2"></div>
</div>
<div class="row">
<div class="cell" symbol="0" x="2" y="0"></div>
<div class="cell" symbol="0" x="2" y="1"></div>
<div class="cell" symbol="0" x="2" y="2"></div>
</div>
</div>
</div>


<div id="toolbar">
<div>
<input type="radio" id="tool_edit"
name="tool_switching" value="edit" checked>
<label for="tool_edit">Edit</label>

<input type="radio" id="tool_select"
name="tool_switching" value="select">
<label for="tool_select">Select</label>

<input type="radio" id="tool_floodfill"
name="tool_switching" value="floodfill">
<label for="tool_floodfill">Flood fill</label>
</div>
</div>

<div id="symbol_picker">
<div class="symbol_preview symbol_0 selected-symbol-preview" symbol="0"></div>
<div class="symbol_preview symbol_1" symbol="1"></div>
<div class="symbol_preview symbol_2" symbol="2"></div>
<div class="symbol_preview symbol_3" symbol="3"></div>
<div class="symbol_preview symbol_4" symbol="4"></div>
<div class="symbol_preview symbol_5" symbol="5"></div>
<div class="symbol_preview symbol_6" symbol="6"></div>
<div class="symbol_preview symbol_7" symbol="7"></div>
<div class="symbol_preview symbol_8" symbol="8"></div>
<div class="symbol_preview symbol_9" symbol="9"></div>
</div>
</div>

<div id="error_display"></div>
<div id="info_display"></div>
</div>
</div>
</div>

<script>
$(function() {
    loadCurrentTask();
    // randomTask();
});
</script>


