# Pipeline Step Fundamentals

## Single-Cell Pipeline Steps

The first step of our workflow is reading data. That section of the notebook
looks like this.

![read data](/images/image7.png){: style="display: block; margin: auto; width:80%"}

This line of code reads the dataset into a `pandas` data frame.

![read data-line](/images/image53.png){: style="display: block; margin: auto; width:80%"}

When working with Kubeflow pipelines, it’s important to be specific about
exactly what code implements a pipeline step and to consider the modules on
which the code depends. We also need to consider the data inputs and outputs
for a given step. 

This step depends on the pandas module. It does not have any inputs since it
is the first step of our pipeline, but it does define and set the variable
`df_auto`, which will be used in many later steps. For purposes of Kubeflow
pipelines, `df_auto` is an output of this pipeline step.

To create our first pipeline step, we’ll do a small amount of reorganizing and
then apply a few annotations.

!!! important "Follow Along"
    Please follow along in your own copy of our notebook as we complete the steps
    below.

### Isolate the code for your step in one cell

Modify your code so that the line that reads the car_prices.csv file is in a
cell by itself.

![read_data step](/images/image63.png){: style="display: block; margin: auto; width:80%"}

### Annotate the cell as a *Pipeline Step* and name it

Click the pencil icon on that cell and set the *Cell type* to *Pipeline Step*
and the *Step name* to `read_data`.

![annotated read_data step](/images/image43.png){: style="display: block; margin: auto; width:80%"}

Click the x to close the annotation editor.

Note that in addition to the label, `read_data`, this cell of our pipeline is
now marked with a vertical line that is the same color as the background of
the label, `read_data`.

If you look more closely, you’ll see that, in fact, all cells below this first
cell have been marked with a vertical line of the same color. 

The default behavior for Kale is that it automatically includes the cells that
follow a step cell as part of the same step until you specify otherwise by
supplying annotations later in the notebook.

In its current state, our entire notebook after the cell in which we read the
`car_prices.csv` file is a single pipeline step. Obviously, we don’t want the
entire notebook to be a single-step pipeline, but this Kale behavior does
provide an important convenience as we’ll see in a moment.

## Multi-Cell Pipeline Steps

The *Clean Data* section of our notebook performs three different tasks to
clean up our dataset. There are several cells here that, together, do the work
of a data cleaning step. 

![clean_data step](/images/image55.png){: style="display: block; margin: auto; width:80%"}

As we hinted in the previous section, Kale enables you to include multiple
cells in a single annotation for a pipeline step.

!!! important "Follow Along"
    Please follow along in your own copy of our notebook as we complete the steps
    below.

Let’s define the second step of our pipeline. As we did before, we need to annotate
a cell with the *Pipeline Step* label. In situations like this where the step is
composed of multiple cells, you’ll want to ensure that all cells are annotated
accordingly.

Annotate the first cell of the data cleaning step and name this step `clean_data`.
The remaining cells for this step will be included in this annotation. If you can’t
remember exactly how to annotate a cell, see the example for `read_data` above or
review the Kale documentation.

When you have finished annotating `clean_data`, that portion of your notebook should
look like the following.

![clean_data step](/images/image58.png){: style="display: block; margin: auto; width:80%"}

## Dependencies Between Pipeline Steps

In order to define a pipeline you need to identify not just the code that makes
up the step, but also specify the order in which the steps of your pipeline
should execute. To do this, select which step (or steps) should immediately
precede the step you are annotating by using the Depends on pull-down menu.

!!! important "Follow Along"
    Please follow along in your own copy of our notebook as we add a dependency
    for clean_data.

The step `clean_data` relies on `read_data` to read our dataset into a `pandas`
data frame (`df_auto`) so we need to define that relationship and establish the
sequence in which these two steps should execute.

![clean_data dependency](/images/image59.png){: style="display: block; margin: auto; width:80%"}

With the work we’ve done so far, we now have a two-step pipeline that we can
summarize as follows.

!!! important "Make sure you understand your data dependencies"
    When considering how to organize your notebook into a Kubeflow pipeline it
    is essential that you assess the data dependencies between steps and ensure
    that values you’ve set for the Depends on field for each step reflect these
    dependencies.