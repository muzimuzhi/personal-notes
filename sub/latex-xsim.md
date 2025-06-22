# LaTeX Cheatsheet: `xsim` package

## Basic use

```latex
\begin{exercise}[<options>]
content
\end{exercise}

\begin{solution}[<options>]
content
\end{solution}

\printsolutions
```

## Concepts

|           |                                        |      |
| --------- | -------------------------------------- | ---- |
| template  | code frameworks                        |      |
| parameter | options of certain "exercise type"     |      |
| property  | options of certain individual exercise |      |

## Templates

Declare new templates

```latex
% environment templates
\DeclareExerciseEnvironmentTemplate{<env tmpl>}{<begin tmpl code>}{<end tmpl code>}

% heading templates
\DeclareExerciseHeadingTemplate{<head tmpl>}{<tmpl code>}

% grading table templates
\DeclareExerciseTableTemplate{<table tmpl>}{<tmpl code>}
````

```
<begin tmpl code> := <tmpl code>
<end tmpl code>   := <tmpl code>
```

Predefined templates

```
<env tmpl>   := default | <user defined>
<head tmpl>  := default | collection | per-section | per-chapter | <user defined>
<table tmpl> := default | default* | <user defined>

```

Special Commands in `<tmlp code>`



## Types of Exercises

```latex
% declare
\DeclareExerciseType    {<type>}{<parameters>}
\SetExerciseParameters  {<type>}{<parameters>}

%

% config

```

Full list of `<parameters>`

```latex
% mandatory
exercise-env  = <exercise env name>,
solution-env  = <solution env name>,
exercise-name = <exercise name>,
solution-name = <solution name>,
exercise-template = <env tmpl>,
solution-template = <env tmpl>,

% optional
counter = <counter>,          % default <exercise env name>
solution-counter = <counter>, % default <solution env name>

% cannot set by user
number = <integer>,           % keep track of the number of exercises of a type
```
