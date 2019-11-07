# Maze Generator - Recursive Back tracker (C#, Visual Basic)
## Requires
- Visual Studio 2015
## License
- MIT
## Technologies
- Graphics
- threading
- Visual Basic .NET
- Visual Studio
- Visual C#
- maze
- Algorithms
- Graphics and Gaming
## Topics
- threading
- maze
- Algorithms
## Updated
- 10/26/2016
## Description

<h1>Introduction</h1>
<p><em>Hello there! I bet you're just dying to know how you see people generating mazes with code, aren't you? Huh-uh-ehh? Yea you are! So was I! So without further adu, don't be shy! Let's dive right in!!!!</em></p>
<p><em><img id="162675" src="162675-dd.png" alt="" width="510" height="637" style="display:block; margin-left:auto; margin-right:auto"><br>
</em></p>
<h1><span>Building the Sample</span></h1>
<p><em>So assuming that you actually want to know how this works, I will begin explaining what steps you need to take, and what each of these steps does to help us accomplish the generation of a maze in Visual Basic!</em></p>
<p><span style="font-size:20px; font-weight:bold">Description</span></p>
<p><em>Writing a program that genrates a maze can be confusing! Especially if you have no clue where to start! This example will generate mazes and spit out a bitmap maze that is N columns wide, and N rows high, and also each cell will be N pixels wide and
 N pixels tall! Bam! Boom! OHHHHH YEAA Maze Land Here I come!!!</em></p>
<p>&nbsp;</p>
<h2><em>Step 1.) - New Project</em></h2>
<ul>
<li>Create a new <em><strong>Windows Forms</strong></em> application(Visual Basic OR C#) - No bias here! Code away in the language
<strong><em>YOU </em></strong>love! </li></ul>
<p>&nbsp;</p>
<h2><em>Step 2.) - References</em></h2>
<ul>
<li><em>If you're using Visual Basic, You're going to want to set the following compiler options:</em>
<ul>
<li><em>Option Strict On</em> </li><li><em>Option Explicit On</em> </li><li><em>Option Infer Off</em> </li></ul>
</li><li><em>Next, this should be done automatically, but just in case, know that this project will require the following references be included with your project, and
<strong>Imported</strong> with Visual Basic(or <strong>Using</strong> with C#):</em>
<ul>
<li><em>System.Threading</em> </li></ul>
</li></ul>
<h2><em>Step 3.) - Visual Design Of Interface</em></h2>
<ul>
<li><em>Add the following Controls to your form</em>
<ul>
<li><em>A button named &quot;Button1&quot;, set the text property to &quot;New Maze&quot;</em> </li><li><em>Four labels with the following text</em>
<ul>
<li><em>Column Count</em>
<ul>
<li><em>Place a NumericUpDown control named &quot;nudCols&quot; under this label(see picture above)</em>
</li></ul>
</li><li><em>Row Count</em>
<ul>
<li><em>Place a NumericUpDown control named &quot;nudRows&quot; under this label(see picture above)</em>
</li></ul>
</li><li><em>Cell Width(pixels)</em> </li></ul>
<ul>
<li>&nbsp;
<ul>
<li><em>Place a NumericUpDown control named &quot;nudWidth&quot; under this label(see picture above)</em>
</li></ul>
</li><li><em>Cell Height(pixels)</em> </li></ul>
<ul>
<li>&nbsp;
<ul>
<li><em>Place a NumericUpDown control named &quot;nudHeight&quot; under this label(see picture above)</em>
</li></ul>
</li></ul>
</li><li><em>A Panel named &quot;Panel2&quot;</em> </li><li><em>A Graphics Panel named &quot;Panel1&quot; - (See Below: This will be explained later)</em>
<ul>
<li><em>Move the graphics panel into &quot;Panel1&quot;</em> </li><li><em>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>Visual Basic</span><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">vb</span><span class="hidden">csharp</span>
<pre class="hidden">Public Class GraphicsPanel
    Inherits Panel
    Sub New()
        Me.DoubleBuffered = True
    End Sub
End Class
</pre>
<pre class="hidden">using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace WindowsFormsApplication1
{
    public class GraphicsPanel : System.Windows.Forms.Panel
    {
        public GraphicsPanel()
        {
            this.DoubleBuffered = true;
        }
    }
}</pre>
<div class="preview">
<pre class="vb"><span class="visualBasic__keyword">Public</span>&nbsp;<span class="visualBasic__keyword">Class</span>&nbsp;GraphicsPanel&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__keyword">Inherits</span>&nbsp;Panel&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__keyword">Sub</span>&nbsp;<span class="visualBasic__keyword">New</span>()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__keyword">Me</span>.DoubleBuffered&nbsp;=&nbsp;<span class="visualBasic__keyword">True</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__keyword">End</span>&nbsp;<span class="visualBasic__keyword">Sub</span>&nbsp;
<span class="visualBasic__keyword">End</span>&nbsp;<span class="visualBasic__keyword">Class</span>&nbsp;
</pre>
</div>
</div>
</div>
</em></li></ul>
</li></ul>
</li></ul>
<p>&nbsp;</p>
<p>That concludes the interface design portion of the tutorial! Amazing! Stunning!</p>
<p>&nbsp;</p>
<p style="padding-left:30px">*In reference to the graphics panel code listed above, this is basically an inherited panel with the doublebuffered property set to true, this allows for faster rendering, although currently in this tutorial the rendering is done
 onto a bitmap.</p>
<p style="padding-left:30px">&nbsp;</p>
<h1>Generating The Maze!</h1>
<p>&nbsp;</p>
<p>If you've made it this far, you're doing GREAT! Actually, this is where we really get started, so hold on to your hats and brace yourself for a wild ride!</p>
<p>Before we dive into code, in laymens terms, I will outline how this works.</p>
<p>As explained on the <a href="https://en.wikipedia.org/wiki/Maze_generation_algorithm" target="_blank">
Wikipedia page - Maze generation algorithm&nbsp;</a>, these are the steps in the recursive backtracker maze generation algorithm.</p>
<p>&nbsp;</p>
<ol>
<li><span style="font-size:small"><em>Make the initial cell the current cell, and mark it as visited</em></span>
</li><li><span style="font-size:small"><em>While there are unvisited cells</em></span>
<ol>
<li>&nbsp;
<ol>
<li><em>Choose randomly one of the unvisited neighbours</em> </li><li><em>Push the current cell to the stack</em> </li><li><em>Remove the wall between the current cell and the chosen cell</em> </li><li><em>Make the chosen cell the current cell and mark it as visited</em> </li></ol>
</li></ol>
<ol>
<li>&nbsp;
<ol>
<li><em>Pop a cell from the stack</em> </li><li><em>Make it the current cell</em> </li></ol>
</li></ol>
<ul>
<li><em>If the current cell has any neighbours which have not been visited</em> </li></ul>
<ul>
<li><em>If the current cell has no unvisited neighbors</em> </li></ul>
</li></ol>
<p>&nbsp;</p>
<p>Can you believe it? That's the entire algorithm! Easy right? Well yes and no. <em>
Now we need to implement this algorithm!</em></p>
<p>&nbsp;</p>
<h2 style="text-align:center"><em>The Cell Class</em></h2>
<p>So for an object oriented approach, we need to have a way to capture each cell in the maze, so we can set the walls on or off, mark as visited and so on.</p>
<p style="text-align:center">&nbsp;</p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>JavaScript</span><span>Visual Basic</span><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">js</span><span class="hidden">vb</span><span class="hidden">csharp</span>
<pre class="hidden">    class Cell
    {
        constructor(location, size, celllist, r, c, maxR, maxC)
        {
            this.Bounds = new Rectangle(location, size);
            this.Column = c;
            this.Row = r;
            this.id = &quot;c&quot; &#43; c.toString() &#43; &quot;r&quot; &#43; r.toString();
            var rowNort = r - 1;
            var rowSout = r &#43; 1;
            var colEast = c &#43; 1;
            var colWest = c - 1;
            this.NeighborNorthID = &quot;c&quot; &#43; c.toString() &#43; &quot;r&quot; &#43; rowNort.toString();
            this.NeighborSouthID = &quot;c&quot; &#43; c.toString() &#43; &quot;r&quot; &#43; rowSout.toString();
            this.NeighborEastID = &quot;c&quot; &#43; colEast.toString() &#43; &quot;r&quot; &#43; r.toString();
            this.NeighborWestID = &quot;c&quot; &#43; colWest.toString() &#43; &quot;r&quot; &#43; r.toString();
            if(rowNort &lt; 0){this.NeighborNorthID = undefined;}
            if(rowSout &gt; maxR){this.NeighborSouthID = undefined;}
            if(colEast &gt; maxC){this.NeighborEastID = undefined;}
            if(colWest &lt; 0){this.NeighborWestID = undefined;}
            this.Cells = celllist;
            this.Cells[this.id] = this;
            this.visited = false;
            this.Stack = [];
            this.NorthWall = true;
            this.SouthWall = true;
            this.WestWall = true;
            this.EastWall = true;
            this.drawLine =  function(g,x1,y1,x2,y2)
            {
                g.beginPath();
                g.strokeStyle=&quot;black&quot;;
                g.moveTo(x1 &#43; .5, y1 &#43; .5);
                g.lineTo(x2 &#43; .5, y2 &#43; .5);
                g.stroke();
            };
            this.draw = function(g)
            {
                if(this.NorthWall){this.drawLine(g,this.Bounds.left ,this.Bounds.top   ,this.Bounds.right,this.Bounds.top);}
                if(this.SouthWall){this.drawLine(g,this.Bounds.left ,this.Bounds.bottom,this.Bounds.right,this.Bounds.bottom);}
                if(this.EastWall) {this.drawLine(g,this.Bounds.right,this.Bounds.top   ,this.Bounds.right,this.Bounds.bottom);}
                if(this.WestWall) {this.drawLine(g,this.Bounds.left ,this.Bounds.top   ,this.Bounds.left ,this.Bounds.bottom);}
            };
            this.getNeighbor = function()
            {
                var unvisitedNeighbors = [];
                var northCell = this.Cells[this.NeighborNorthID];
                var southCell = this.Cells[this.NeighborSouthID];
                var eastCell = this.Cells[this.NeighborEastID];
                var westCell = this.Cells[this.NeighborWestID];
                if(northCell!= undefined){if(northCell.visited==false){unvisitedNeighbors.push(northCell);}}
                if(southCell!= undefined){if(southCell.visited==false){unvisitedNeighbors.push(southCell);}}
                if(eastCell != undefined){if(eastCell.visited ==false){unvisitedNeighbors.push(eastCell);}}
                if(westCell != undefined){if(westCell.visited ==false){unvisitedNeighbors.push(westCell);}}
                var max = unvisitedNeighbors.length-1;
                var currentCell = undefined;
                if(unvisitedNeighbors.length &gt; 0)
                {
                    var index = rand(0, max);
                    currentCell = unvisitedNeighbors[index];
                }
                return currentCell;
            };
            this.Dig = function(Stack)
            {
                this.Stack = Stack;
                var nextCell = this.getNeighbor();
                if(nextCell != undefined)
                {
                    this.Stack.push(nextCell);
                    switch(true)
                    {
                        case(nextCell.id == this.NeighborNorthID):
                            this.NorthWall = false;
                            nextCell.SouthWall = false;
                            break;
                        case(nextCell.id == this.NeighborSouthID):
                            this.SouthWall = false;
                            nextCell.NorthWall = false;
                            break;
                        case(nextCell.id == this.NeighborEastID):
                            this.EastWall = false;
                            nextCell.WestWall = false;
                            break;
                        case(nextCell.id == this.NeighborWestID):
                            this.WestWall = false;
                            nextCell.EastWall = false;
                            break;
                    }
                }
                else if(this.Stack.length != 0)
                {
                    nextCell = this.Stack.pop();
                }
                return nextCell;
            };
        }
    }</pre>
<pre class="hidden">Public Class Cell
    Public NorthWall As Boolean = True
    Public SouthWall As Boolean = True
    Public WestWall As Boolean = True
    Public EastWall As Boolean = True
    Public id As String '
    Public Pen As Pen = Pens.Black
    Public Bounds As Rectangle '
    Public Cells As Dictionary(Of String, Cell) '
    Public Column As Integer '
    Public Row As Integer '
    Public NeighborNorthID As String '
    Public NeighborSouthID As String '
    Public NeighborEastID As String '
    Public NeighborWestID As String '
    Public Visited As Boolean = False '
    Public Stack As Stack(Of Cell) '
    Public Sub draw(g As Graphics)
        If NorthWall Then g.DrawLine(Pen, New Point(Bounds.Left, Bounds.Top), New Point(Bounds.Right, Bounds.Top))
        If SouthWall Then g.DrawLine(Pen, New Point(Bounds.Left, Bounds.Bottom), New Point(Bounds.Right, Bounds.Bottom))
        If WestWall Then g.DrawLine(Pen, New Point(Bounds.Left, Bounds.Top), New Point(Bounds.Left, Bounds.Bottom))
        If EastWall Then g.DrawLine(Pen, New Point(Bounds.Right, Bounds.Top), New Point(Bounds.Right, Bounds.Bottom))
    End Sub
    Sub New(location As Point, size As Size, ByRef cellList As Dictionary(Of String, Cell), r As Integer, c As Integer, maxR As Integer, maxC As Integer)
        Me.Bounds = New Rectangle(location, size)
        Me.Column = c
        Me.Row = r
        Me.id = &quot;c&quot; &amp; c &amp; &quot;r&quot; &amp; r
        Dim rowNort As Integer = r - 1
        Dim rowSout As Integer = r &#43; 1
        Dim colEast As Integer = c &#43; 1
        Dim colWest As Integer = c - 1
        NeighborNorthID = &quot;c&quot; &amp; c &amp; &quot;r&quot; &amp; rowNort
        NeighborSouthID = &quot;c&quot; &amp; c &amp; &quot;r&quot; &amp; rowSout
        NeighborEastID = &quot;c&quot; &amp; colEast &amp; &quot;r&quot; &amp; r
        NeighborWestID = &quot;c&quot; &amp; colWest &amp; &quot;r&quot; &amp; r
        If rowNort &lt; 0 Then NeighborNorthID = &quot;none&quot;
        If rowSout &gt; maxR Then NeighborSouthID = &quot;none&quot;
        If colEast &gt; maxC Then NeighborEastID = &quot;none&quot;
        If colWest &lt; 0 Then NeighborWestID = &quot;none&quot;
        Me.Cells = cellList
        Me.Cells.Add(Me.id, Me)
    End Sub
    Function getNeighbor() As Cell
        Dim c As New List(Of Cell)
        If Not NeighborNorthID = &quot;none&quot; AndAlso Cells(NeighborNorthID).Visited = False Then c.Add(Cells(NeighborNorthID))
        If Not NeighborSouthID = &quot;none&quot; AndAlso Cells(NeighborSouthID).Visited = False Then c.Add(Cells(NeighborSouthID))
        If Not NeighborEastID = &quot;none&quot; AndAlso Cells(NeighborEastID).Visited = False Then c.Add(Cells(NeighborEastID))
        If Not NeighborWestID = &quot;none&quot; AndAlso Cells(NeighborWestID).Visited = False Then c.Add(Cells(NeighborWestID))
        Dim max As Integer = c.Count
        Dim currentCell As Cell = Nothing
        If c.Count &gt; 0 Then
            Randomize()
            Dim index As Integer = CInt(Int(c.Count * Rnd()))
            currentCell = c(index)
        End If
        Return currentCell
    End Function
    Function Dig(ByRef stack As Stack(Of Cell)) As Cell
        Me.Stack = stack
        Dim nextCell As Cell = getNeighbor()
        If Not nextCell Is Nothing Then
            stack.Push(nextCell)
            If nextCell.id = Me.NeighborNorthID Then
                Me.NorthWall = False
                nextCell.SouthWall = False
            ElseIf nextCell.id = Me.NeighborSouthID
                Me.SouthWall = False
                nextCell.NorthWall = False
            ElseIf nextCell.id = Me.NeighborEastID
                Me.EastWall = False
                nextCell.WestWall = False
            ElseIf nextCell.id = Me.NeighborWestID
                Me.WestWall = False
                nextCell.EastWall = False
            End If
        ElseIf Not stack.Count = 0
            nextCell = stack.Pop
        Else
            Return Nothing
        End If
        Return nextCell
    End Function
End Class
</pre>
<pre class="hidden">public class Cell
{
    public bool NorthWall = true;
    public bool SouthWall = true;
    public bool WestWall = true;
    public bool EastWall = true;
    public string id;
    public Pen Pen = Pens.Black;
    public Rectangle Bounds;
    public Dictionary&lt;string, Cell&gt; Cells;
    public int Column;
    public int Row;
    public string NeighborNorthID;
    public string NeighborSouthID;
    public string NeighborEastID;
    public string NeighborWestID;
    public bool Visited = false;
    public Stack&lt;Cell&gt; Stack;
    public void draw(Graphics g)
    {
        if (NorthWall) g.DrawLine(Pen, new Point(Bounds.Left, Bounds.Top), new Point(Bounds.Right, Bounds.Top));
        if (SouthWall)g.DrawLine(Pen, new Point(Bounds.Left, Bounds.Bottom), new Point(Bounds.Right, Bounds.Bottom));
        if (WestWall)g.DrawLine(Pen, new Point(Bounds.Left, Bounds.Top), new Point(Bounds.Left, Bounds.Bottom));
        if (EastWall)g.DrawLine(Pen, new Point(Bounds.Right, Bounds.Top), new Point(Bounds.Right, Bounds.Bottom));
    }
    public Cell(Point location, Size size, ref Dictionary&lt;string, Cell&gt; cellList, int r, int c, int maxR, int maxC)
    {
        this.Bounds = new Rectangle(location, size);
        this.Column = c;
        this.Row = r;
        this.id = &quot;c&quot; &#43; c &#43; &quot;r&quot; &#43; r;
        int rowNort = r - 1;
        int rowSout = r &#43; 1;
        int colEast = c &#43; 1;
        int colWest = c - 1;
        NeighborNorthID = &quot;c&quot; &#43; c &#43; &quot;r&quot; &#43; rowNort;
        NeighborSouthID = &quot;c&quot; &#43; c &#43; &quot;r&quot; &#43; rowSout;
        NeighborEastID = &quot;c&quot; &#43; colEast &#43; &quot;r&quot; &#43; r;
        NeighborWestID = &quot;c&quot; &#43; colWest &#43; &quot;r&quot; &#43; r;
        if (rowNort &lt; 0) NeighborNorthID = &quot;none&quot;;
        if (rowSout &gt; maxR) NeighborSouthID = &quot;none&quot;;
        if (colEast &gt; maxC) NeighborEastID = &quot;none&quot;;
        if (colWest &lt; 0) NeighborWestID = &quot;none&quot;;
        this.Cells = cellList;
        this.Cells.Add(this.id, this);
    }
    public Cell getNeighbor()
    {
        List&lt;Cell&gt; c = new List&lt;Cell&gt;();
        if (!(NeighborNorthID == &quot;none&quot;) &amp;&amp; Cells[NeighborNorthID].Visited == false)c.Add(Cells[NeighborNorthID]);
        if (!(NeighborSouthID == &quot;none&quot;) &amp;&amp; Cells[NeighborSouthID].Visited == false)c.Add(Cells[NeighborSouthID]);
        if (!(NeighborEastID == &quot;none&quot;) &amp;&amp; Cells[NeighborEastID].Visited == false)c.Add(Cells[NeighborEastID]);
        if (!(NeighborWestID == &quot;none&quot;) &amp;&amp; Cells[NeighborWestID].Visited == false)c.Add(Cells[NeighborWestID]);
        int max = c.Count;
        Cell currentCell = null;
        if (c.Count &gt; 0)
        {
            Microsoft.VisualBasic.VBMath.Randomize();
            int index = Convert.ToInt32(Conversion.Int(c.Count * VBMath.Rnd()));
            currentCell = c[index];
        }
        return currentCell;
    }
    public Cell Dig(ref Stack&lt;Cell&gt; stack)
    {
        this.Stack = stack;
        Cell nextCell = getNeighbor();
        if ((nextCell != null))
        {
            stack.Push(nextCell);
            if (nextCell.id == this.NeighborNorthID)
            {
                this.NorthWall = false;
                nextCell.SouthWall = false;
            }
            else if (nextCell.id == this.NeighborSouthID)
            {
                this.SouthWall = false;
                nextCell.NorthWall = false;
            }
            else if (nextCell.id == this.NeighborEastID)
            {
                this.EastWall = false;
                nextCell.WestWall = false;
            }
            else if (nextCell.id == this.NeighborWestID)
            {
                this.WestWall = false;
                nextCell.EastWall = false;
            }
        }
        else if (!(stack.Count == 0))
        {
            nextCell = stack.Pop();
        }
        else
        {
            return null;
        }
        return nextCell;
    }
}</pre>
<div class="preview">
<pre class="js">&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Cell&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(location,&nbsp;size,&nbsp;celllist,&nbsp;r,&nbsp;c,&nbsp;maxR,&nbsp;maxC)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Bounds&nbsp;=&nbsp;<span class="js__operator">new</span>&nbsp;Rectangle(location,&nbsp;size);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Column&nbsp;=&nbsp;c;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Row&nbsp;=&nbsp;r;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.id&nbsp;=&nbsp;<span class="js__string">&quot;c&quot;</span>&nbsp;&#43;&nbsp;c.toString()&nbsp;&#43;&nbsp;<span class="js__string">&quot;r&quot;</span>&nbsp;&#43;&nbsp;r.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;rowNort&nbsp;=&nbsp;r&nbsp;-&nbsp;<span class="js__num">1</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;rowSout&nbsp;=&nbsp;r&nbsp;&#43;&nbsp;<span class="js__num">1</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;colEast&nbsp;=&nbsp;c&nbsp;&#43;&nbsp;<span class="js__num">1</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;colWest&nbsp;=&nbsp;c&nbsp;-&nbsp;<span class="js__num">1</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.NeighborNorthID&nbsp;=&nbsp;<span class="js__string">&quot;c&quot;</span>&nbsp;&#43;&nbsp;c.toString()&nbsp;&#43;&nbsp;<span class="js__string">&quot;r&quot;</span>&nbsp;&#43;&nbsp;rowNort.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.NeighborSouthID&nbsp;=&nbsp;<span class="js__string">&quot;c&quot;</span>&nbsp;&#43;&nbsp;c.toString()&nbsp;&#43;&nbsp;<span class="js__string">&quot;r&quot;</span>&nbsp;&#43;&nbsp;rowSout.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.NeighborEastID&nbsp;=&nbsp;<span class="js__string">&quot;c&quot;</span>&nbsp;&#43;&nbsp;colEast.toString()&nbsp;&#43;&nbsp;<span class="js__string">&quot;r&quot;</span>&nbsp;&#43;&nbsp;r.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.NeighborWestID&nbsp;=&nbsp;<span class="js__string">&quot;c&quot;</span>&nbsp;&#43;&nbsp;colWest.toString()&nbsp;&#43;&nbsp;<span class="js__string">&quot;r&quot;</span>&nbsp;&#43;&nbsp;r.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(rowNort&nbsp;&lt;&nbsp;<span class="js__num">0</span>)<span class="js__brace">{</span><span class="js__operator">this</span>.NeighborNorthID&nbsp;=&nbsp;<span class="js__property">undefined</span>;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(rowSout&nbsp;&gt;&nbsp;maxR)<span class="js__brace">{</span><span class="js__operator">this</span>.NeighborSouthID&nbsp;=&nbsp;<span class="js__property">undefined</span>;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(colEast&nbsp;&gt;&nbsp;maxC)<span class="js__brace">{</span><span class="js__operator">this</span>.NeighborEastID&nbsp;=&nbsp;<span class="js__property">undefined</span>;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(colWest&nbsp;&lt;&nbsp;<span class="js__num">0</span>)<span class="js__brace">{</span><span class="js__operator">this</span>.NeighborWestID&nbsp;=&nbsp;<span class="js__property">undefined</span>;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Cells&nbsp;=&nbsp;celllist;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Cells[<span class="js__operator">this</span>.id]&nbsp;=&nbsp;<span class="js__operator">this</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.visited&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Stack&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.NorthWall&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.SouthWall&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.WestWall&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.EastWall&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.drawLine&nbsp;=&nbsp;&nbsp;<span class="js__operator">function</span>(g,x1,y1,x2,y2)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.beginPath();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.strokeStyle=<span class="js__string">&quot;black&quot;</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.moveTo(x1&nbsp;&#43;&nbsp;.<span class="js__num">5</span>,&nbsp;y1&nbsp;&#43;&nbsp;.<span class="js__num">5</span>);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.lineTo(x2&nbsp;&#43;&nbsp;.<span class="js__num">5</span>,&nbsp;y2&nbsp;&#43;&nbsp;.<span class="js__num">5</span>);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.stroke();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.draw&nbsp;=&nbsp;<span class="js__operator">function</span>(g)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(<span class="js__operator">this</span>.NorthWall)<span class="js__brace">{</span><span class="js__operator">this</span>.drawLine(g,<span class="js__operator">this</span>.Bounds.left&nbsp;,<span class="js__operator">this</span>.Bounds.top&nbsp;&nbsp;&nbsp;,<span class="js__operator">this</span>.Bounds.right,<span class="js__operator">this</span>.Bounds.top);<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(<span class="js__operator">this</span>.SouthWall)<span class="js__brace">{</span><span class="js__operator">this</span>.drawLine(g,<span class="js__operator">this</span>.Bounds.left&nbsp;,<span class="js__operator">this</span>.Bounds.bottom,<span class="js__operator">this</span>.Bounds.right,<span class="js__operator">this</span>.Bounds.bottom);<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(<span class="js__operator">this</span>.EastWall)&nbsp;<span class="js__brace">{</span><span class="js__operator">this</span>.drawLine(g,<span class="js__operator">this</span>.Bounds.right,<span class="js__operator">this</span>.Bounds.top&nbsp;&nbsp;&nbsp;,<span class="js__operator">this</span>.Bounds.right,<span class="js__operator">this</span>.Bounds.bottom);<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(<span class="js__operator">this</span>.WestWall)&nbsp;<span class="js__brace">{</span><span class="js__operator">this</span>.drawLine(g,<span class="js__operator">this</span>.Bounds.left&nbsp;,<span class="js__operator">this</span>.Bounds.top&nbsp;&nbsp;&nbsp;,<span class="js__operator">this</span>.Bounds.left&nbsp;,<span class="js__operator">this</span>.Bounds.bottom);<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.getNeighbor&nbsp;=&nbsp;<span class="js__operator">function</span>()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;unvisitedNeighbors&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;northCell&nbsp;=&nbsp;<span class="js__operator">this</span>.Cells[<span class="js__operator">this</span>.NeighborNorthID];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;southCell&nbsp;=&nbsp;<span class="js__operator">this</span>.Cells[<span class="js__operator">this</span>.NeighborSouthID];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;eastCell&nbsp;=&nbsp;<span class="js__operator">this</span>.Cells[<span class="js__operator">this</span>.NeighborEastID];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;westCell&nbsp;=&nbsp;<span class="js__operator">this</span>.Cells[<span class="js__operator">this</span>.NeighborWestID];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(northCell!=&nbsp;<span class="js__property">undefined</span>)<span class="js__brace">{</span><span class="js__statement">if</span>(northCell.visited==false)<span class="js__brace">{</span>unvisitedNeighbors.push(northCell);<span class="js__brace">}</span><span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(southCell!=&nbsp;<span class="js__property">undefined</span>)<span class="js__brace">{</span><span class="js__statement">if</span>(southCell.visited==false)<span class="js__brace">{</span>unvisitedNeighbors.push(southCell);<span class="js__brace">}</span><span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(eastCell&nbsp;!=&nbsp;<span class="js__property">undefined</span>)<span class="js__brace">{</span><span class="js__statement">if</span>(eastCell.visited&nbsp;==false)<span class="js__brace">{</span>unvisitedNeighbors.push(eastCell);<span class="js__brace">}</span><span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(westCell&nbsp;!=&nbsp;<span class="js__property">undefined</span>)<span class="js__brace">{</span><span class="js__statement">if</span>(westCell.visited&nbsp;==false)<span class="js__brace">{</span>unvisitedNeighbors.push(westCell);<span class="js__brace">}</span><span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;max&nbsp;=&nbsp;unvisitedNeighbors.length<span class="js__num">-1</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;currentCell&nbsp;=&nbsp;<span class="js__property">undefined</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(unvisitedNeighbors.length&nbsp;&gt;&nbsp;<span class="js__num">0</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;index&nbsp;=&nbsp;rand(<span class="js__num">0</span>,&nbsp;max);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;currentCell&nbsp;=&nbsp;unvisitedNeighbors[index];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">return</span>&nbsp;currentCell;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Dig&nbsp;=&nbsp;<span class="js__operator">function</span>(Stack)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Stack&nbsp;=&nbsp;Stack;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;nextCell&nbsp;=&nbsp;<span class="js__operator">this</span>.getNeighbor();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(nextCell&nbsp;!=&nbsp;<span class="js__property">undefined</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Stack.push(nextCell);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">switch</span>(true)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">case</span>(nextCell.id&nbsp;==&nbsp;<span class="js__operator">this</span>.NeighborNorthID):&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.NorthWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell.SouthWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">break</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">case</span>(nextCell.id&nbsp;==&nbsp;<span class="js__operator">this</span>.NeighborSouthID):&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.SouthWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell.NorthWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">break</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">case</span>(nextCell.id&nbsp;==&nbsp;<span class="js__operator">this</span>.NeighborEastID):&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.EastWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell.WestWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">break</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">case</span>(nextCell.id&nbsp;==&nbsp;<span class="js__operator">this</span>.NeighborWestID):&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.WestWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell.EastWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">break</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">else</span>&nbsp;<span class="js__statement">if</span>(<span class="js__operator">this</span>.Stack.length&nbsp;!=&nbsp;<span class="js__num">0</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell&nbsp;=&nbsp;<span class="js__operator">this</span>.Stack.pop();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">return</span>&nbsp;nextCell;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span></pre>
</div>
</div>
</div>
<p>&nbsp;</p>
<h2 style="text-align:center">The Maze Class</h2>
<p>&nbsp;</p>
<p>Next we create a class that contains all of the functionality of the maze generation algorithm</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>JavaScript</span><span>Visual Basic</span><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">js</span><span class="hidden">vb</span><span class="hidden">csharp</span>
<pre class="hidden">    class Maze
    {
        constructor(rows, columns, cellWidth, cellHeight)
        {
            this.Rows = rows;
            this.Columns = columns;
            this.cellWidth = cellWidth;
            this.cellHeight = cellHeight;
            this.Width = (this.Columns * this.cellWidth) &#43; 1;
            this.Height = (this.Rows * this.cellHeight) &#43; 1;
            this.cells = [];
            this.Stack = [];
            this.bounds = function()
            {
                var rect = new Rectangle(new Point(0,0), new Size(this.Width, this.Height))
                return rect;
            };
            this.Generate = function()
            {
                var c = 0;
                var r = 0;
                for(var y=0;y&lt;=this.Height;y&#43;=this.cellHeight)
                {
                    for(var x=0;x&lt;=this.Width;x&#43;=this.cellWidth)
                    {
                        var cell = new Cell(new Point(x, y), new Size(this.cellWidth, this.cellHeight), this.cells, r, c, (this.Rows - 1), (this.Columns - 1))
                       
                        c&#43;&#43;;
                    }
                    c=0;r&#43;&#43;;
                }
                this.Dig();
            };
            this.Dig = function()
            {
                var r = 0;
                var c = 0;
                var key = &quot;c0r0&quot;;
                var startCell = this.cells[key];
                this.Stack = [];
                startCell.visited = true;
                var visitedCells = 0;
                var counter= 0;
                while(startCell != undefined)
                {
                    startCell = startCell.Dig(this.Stack);
                    if(startCell != undefined)
                    {
                        startCell.visited = true;
                    }
                    counter&#43;&#43;;
                }
                this.Stack = [];
                var c = document.getElementById('MyCanvas');
                c.width = this.Width;
                c.height = this.Height;
                var g = c.getContext(&quot;2d&quot;);
                g.fillStyle=&quot;aqua&quot;;
                g.fillRect(0, 0, this.Width, this.Height);
                var size = Object.keys(this.cells).length;
                if(size &gt; 0)
                {
                    for(r = 0; r &lt;= this.Rows; r&#43;&#43;)
                    {
                        for(c = 0; c &lt;= this.Columns; c&#43;&#43;)
                        {
                            var id = &quot;c&quot; &#43; c &#43; &quot;r&quot; &#43; r;
                            var cell = this.cells[id];
                            cell.draw(g);
                        }
                    }
                }
                this.cells.length=0;
            };
        }
    }</pre>
<pre class="hidden">Public Class Maze
    Inherits Control
    Dim Rows As Integer
    Dim Columns As Integer
    Dim cellWidth As Integer
    Dim cellHeight As Integer
    Dim cells As New Dictionary(Of String, Cell)
    Dim stack As New Stack(Of Cell)
    Public Maze As Image

    Public Event MazeComplete(Maze As Image)
    Private Event CallComplete(Maze As Image)
    Public Shadows ReadOnly Property Bounds As Rectangle
        Get
            Dim rect As New Rectangle(0, 0, Width, Height)
            Return rect
        End Get
    End Property
    Dim WithEvents printDoc As New Printing.PrintDocument()
    Private Sub PrintImage(ByVal sender As Object, ByVal e As System.Drawing.Printing.PrintPageEventArgs) Handles printDoc.PrintPage
        Dim nonprinters As List(Of String) = ({&quot;Send To OneNote 2013&quot;, &quot;PDFCreator&quot;, &quot;PDF Architect 4&quot;,
                               &quot;Microsoft XPS Document Writer&quot;, &quot;Microsoft Print to PDF&quot;, &quot;Fax&quot;, &quot;-&quot;}).ToList
        Dim printerName As String = &quot;none&quot;
        For Each a As String In System.Drawing.Printing.PrinterSettings.InstalledPrinters
            If nonprinters.IndexOf(a) &gt; -1 Then Continue For
            printerName = a
        Next
        If printerName = &quot;none&quot; Then Exit Sub
        printDoc.PrinterSettings.PrinterName = printerName
        Dim imageLeft As Integer = CInt(e.PageBounds.Width / 2) - CInt(Maze.Width / 2)
        Dim imageTop As Integer = CInt(e.PageBounds.Height / 2) - CInt(Maze.Height / 2)
        e.Graphics.DrawImage(Maze, imageLeft, imageTop)
    End Sub
    Public Sub PrintMaze()
        printDoc.Print()
    End Sub
    Public Sub Generate()
        Dim c As Integer = 0
        Dim r As Integer = 0
        For y As Integer = 0 To Height Step cellHeight
            For x As Integer = 0 To Width Step cellWidth
                Dim cell As New Cell(New Point(x, y), New Size(cellWidth, cellHeight), cells, r, c, (Rows - 1), (Columns - 1))
                c &#43;= 1
            Next
            c = 0 : r &#43;= 1
        Next
        Dim thread As New Threading.Thread(AddressOf Dig)
        thread.Start()
    End Sub
    Private Sub Dig()
        Dim r As Integer = 0
        Dim c As Integer = 0
        Dim key As String = &quot;c&quot; &amp; 5 &amp; &quot;r&quot; &amp; 5
        Dim startCell As Cell = cells(key)
        stack.Clear()
        startCell.Visited = True
        While (startCell IsNot Nothing)
            startCell = startCell.Dig(stack)
            If startCell IsNot Nothing Then
                startCell.Visited = True
                startCell.Pen = Pens.Black
            End If
        End While
        stack.Clear()
        Dim Maze As New Bitmap(Width, Height)
        Using g As Graphics = Graphics.FromImage(Maze)
            g.Clear(Color.White)
            If cells.Count &gt; 0 Then
                For r = 0 To Me.Rows - 1
                    For c = 0 To Me.Columns - 1
                        Dim cell As Cell = cells(&quot;c&quot; &amp; c &amp; &quot;r&quot; &amp; r)
                        cell.draw(g)
                    Next
                Next
            End If
        End Using
        Me.Maze = Maze

        RaiseEvent CallComplete(Maze)
    End Sub
    Delegate Sub dComplete(maze As Image)
    Private Sub Call_Complete(maze As Image) Handles Me.CallComplete
        If Me.InvokeRequired Then
            Me.Invoke(New dComplete(AddressOf Call_Complete), maze)
        Else
            RaiseEvent MazeComplete(maze)
        End If
    End Sub
    Sub New(rows As Integer, columns As Integer, cellWidth As Integer, cellHeight As Integer)
        Me.Rows = rows
        Me.Columns = columns
        Me.cellWidth = cellWidth
        Me.cellHeight = cellHeight
        Me.Width = (Me.Columns * Me.cellWidth) &#43; 1
        Me.Height = (Me.Rows * Me.cellHeight) &#43; 1
        Me.CreateHandle()
    End Sub
End Class</pre>
<pre class="hidden">public class Maze : Control
{
    int Rows;
    int Columns;
    int cellWidth;
    int cellHeight;
    Dictionary&lt;string, Cell&gt; cells = new Dictionary&lt;string, Cell&gt;();
    Stack&lt;Cell&gt; stack = new Stack&lt;Cell&gt;();

    public Image MazeImage;
    public event MazeCompleteEventHandler MazeComplete;
    public delegate void MazeCompleteEventHandler(Image Maze);
    private event CallCompleteEventHandler CallComplete;
    private delegate void CallCompleteEventHandler(Image Maze);
    public new Rectangle Bounds
    {
        get
        {
            Rectangle rect = new Rectangle(0, 0, Width, Height);
            return rect;
        }
    }
    private System.Drawing.Printing.PrintDocument withEventsField_printDoc = new System.Drawing.Printing.PrintDocument();
    public System.Drawing.Printing.PrintDocument printDoc
    {
        get { return withEventsField_printDoc; }
        set
        {
            if (withEventsField_printDoc != null)
            {
                withEventsField_printDoc.PrintPage -= PrintImage;
            }
            withEventsField_printDoc = value;
            if (withEventsField_printDoc != null)
            {
                withEventsField_printDoc.PrintPage &#43;= PrintImage;
            }
        }
    }
    private void PrintImage(object sender, System.Drawing.Printing.PrintPageEventArgs e)
    {
        List&lt;string&gt; nonprinters = new string[] { &quot;Send To OneNote 2013&quot;, &quot;PDFCreator&quot;, &quot;PDF Architect 4&quot;, &quot;Microsoft XPS Document Writer&quot;, &quot;Microsoft Print to PDF&quot;, &quot;Fax&quot;, &quot;-&quot; }.ToList();
        string printerName = &quot;none&quot;;
        foreach (string a in System.Drawing.Printing.PrinterSettings.InstalledPrinters)
        {
            if (nonprinters.IndexOf(a) &gt; -1)
                continue;
            printerName = a;
        }
        if (printerName == &quot;none&quot;)
            return;
        printDoc.PrinterSettings.PrinterName = printerName;
        int imageLeft = Convert.ToInt32(e.PageBounds.Width / 2) - Convert.ToInt32(MazeImage.Width / 2);
        int imageTop = Convert.ToInt32(e.PageBounds.Height / 2) - Convert.ToInt32(MazeImage.Height / 2);
        e.Graphics.DrawImage(MazeImage, imageLeft, imageTop);
    }
    public void PrintMaze()
    {
        printDoc.Print();
    }
    public void Generate()
    {
        int c = 0;
        int r = 0;
        for (int y = 0; y &lt;= Height; y &#43;= cellHeight)
        {
            for (int x = 0; x &lt;= Width; x &#43;= cellWidth)
            {
                Cell cell = new Cell(new Point(x, y), new Size(cellWidth, cellHeight),ref cells, r, c, (Rows - 1), (Columns - 1));
                c &#43;= 1;
            }
            c = 0;
            r &#43;= 1;
        }
        System.Threading.Thread thread = new System.Threading.Thread(Dig);
        thread.Start();
    }
    private void Dig()
    {
        int r = 0;
        int c = 0;
        string key = &quot;c&quot; &#43; 5 &#43; &quot;r&quot; &#43; 5;
        Cell startCell = cells[key];
        stack.Clear();
        startCell.Visited = true;
        while ((startCell != null))
        {
            startCell = startCell.Dig(ref stack);
            if (startCell != null)
            {
                startCell.Visited = true;
                startCell.Pen = Pens.Black;
            }
        }
        stack.Clear();
        Bitmap Maze = new Bitmap(Width, Height);
        using (Graphics g = Graphics.FromImage(Maze))
        {
            g.Clear(Color.White);
            if (cells.Count &gt; 0)
            {
                for (r = 0; r &lt;= this.Rows - 1; r&#43;&#43;)
                {
                    for (c = 0; c &lt;= this.Columns - 1; c&#43;&#43;)
                    {
                        Cell cell = cells[&quot;c&quot; &#43; c &#43; &quot;r&quot; &#43; r];
                        cell.draw(g);
                    }
                }
            }
        }
        this.MazeImage = Maze;

        if (CallComplete != null)
        {
            CallComplete(Maze);
        }
    }
    public delegate void dComplete(Image maze);
    private void Call_Complete(Image maze)
    {
        if (this.InvokeRequired)
        {
            this.Invoke(new dComplete(Call_Complete), maze);
        }
        else
        {
            if (MazeComplete != null)
            {
                MazeComplete(maze);
            }
        }
    }
    public Maze(int rows, int columns, int cellWidth, int cellHeight)
    {
        CallComplete &#43;= Call_Complete;
        this.Rows = rows;
        this.Columns = columns;
        this.cellWidth = cellWidth;
        this.cellHeight = cellHeight;
        this.Width = (this.Columns * this.cellWidth) &#43; 1;
        this.Height = (this.Rows * this.cellHeight) &#43; 1;
        this.CreateHandle();
    }
}</pre>
<div class="preview">
<pre class="js">&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Maze&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(rows,&nbsp;columns,&nbsp;cellWidth,&nbsp;cellHeight)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Rows&nbsp;=&nbsp;rows;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Columns&nbsp;=&nbsp;columns;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.cellWidth&nbsp;=&nbsp;cellWidth;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.cellHeight&nbsp;=&nbsp;cellHeight;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Width&nbsp;=&nbsp;(<span class="js__operator">this</span>.Columns&nbsp;*&nbsp;<span class="js__operator">this</span>.cellWidth)&nbsp;&#43;&nbsp;<span class="js__num">1</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Height&nbsp;=&nbsp;(<span class="js__operator">this</span>.Rows&nbsp;*&nbsp;<span class="js__operator">this</span>.cellHeight)&nbsp;&#43;&nbsp;<span class="js__num">1</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.cells&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Stack&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.bounds&nbsp;=&nbsp;<span class="js__operator">function</span>()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;rect&nbsp;=&nbsp;<span class="js__operator">new</span>&nbsp;Rectangle(<span class="js__operator">new</span>&nbsp;Point(<span class="js__num">0</span>,<span class="js__num">0</span>),&nbsp;<span class="js__operator">new</span>&nbsp;Size(<span class="js__operator">this</span>.Width,&nbsp;<span class="js__operator">this</span>.Height))&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">return</span>&nbsp;rect;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Generate&nbsp;=&nbsp;<span class="js__operator">function</span>()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;c&nbsp;=&nbsp;<span class="js__num">0</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;r&nbsp;=&nbsp;<span class="js__num">0</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">for</span>(<span class="js__statement">var</span>&nbsp;y=<span class="js__num">0</span>;y&lt;=<span class="js__operator">this</span>.Height;y&#43;=<span class="js__operator">this</span>.cellHeight)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">for</span>(<span class="js__statement">var</span>&nbsp;x=<span class="js__num">0</span>;x&lt;=<span class="js__operator">this</span>.Width;x&#43;=<span class="js__operator">this</span>.cellWidth)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;cell&nbsp;=&nbsp;<span class="js__operator">new</span>&nbsp;Cell(<span class="js__operator">new</span>&nbsp;Point(x,&nbsp;y),&nbsp;<span class="js__operator">new</span>&nbsp;Size(<span class="js__operator">this</span>.cellWidth,&nbsp;<span class="js__operator">this</span>.cellHeight),&nbsp;<span class="js__operator">this</span>.cells,&nbsp;r,&nbsp;c,&nbsp;(<span class="js__operator">this</span>.Rows&nbsp;-&nbsp;<span class="js__num">1</span>),&nbsp;(<span class="js__operator">this</span>.Columns&nbsp;-&nbsp;<span class="js__num">1</span>))&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c&#43;&#43;;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c=<span class="js__num">0</span>;r&#43;&#43;;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Dig();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Dig&nbsp;=&nbsp;<span class="js__operator">function</span>()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;r&nbsp;=&nbsp;<span class="js__num">0</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;c&nbsp;=&nbsp;<span class="js__num">0</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;key&nbsp;=&nbsp;<span class="js__string">&quot;c0r0&quot;</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;startCell&nbsp;=&nbsp;<span class="js__operator">this</span>.cells[key];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Stack&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;startCell.visited&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;visitedCells&nbsp;=&nbsp;<span class="js__num">0</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;counter=&nbsp;<span class="js__num">0</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">while</span>(startCell&nbsp;!=&nbsp;<span class="js__property">undefined</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;startCell&nbsp;=&nbsp;startCell.Dig(<span class="js__operator">this</span>.Stack);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(startCell&nbsp;!=&nbsp;<span class="js__property">undefined</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;startCell.visited&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;counter&#43;&#43;;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.Stack&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;c&nbsp;=&nbsp;document.getElementById(<span class="js__string">'MyCanvas'</span>);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c.width&nbsp;=&nbsp;<span class="js__operator">this</span>.Width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c.height&nbsp;=&nbsp;<span class="js__operator">this</span>.Height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;g&nbsp;=&nbsp;c.getContext(<span class="js__string">&quot;2d&quot;</span>);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.fillStyle=<span class="js__string">&quot;aqua&quot;</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.fillRect(<span class="js__num">0</span>,&nbsp;<span class="js__num">0</span>,&nbsp;<span class="js__operator">this</span>.Width,&nbsp;<span class="js__operator">this</span>.Height);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;size&nbsp;=&nbsp;<span class="js__object">Object</span>.keys(<span class="js__operator">this</span>.cells).length;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">if</span>(size&nbsp;&gt;&nbsp;<span class="js__num">0</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">for</span>(r&nbsp;=&nbsp;<span class="js__num">0</span>;&nbsp;r&nbsp;&lt;=&nbsp;<span class="js__operator">this</span>.Rows;&nbsp;r&#43;&#43;)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">for</span>(c&nbsp;=&nbsp;<span class="js__num">0</span>;&nbsp;c&nbsp;&lt;=&nbsp;<span class="js__operator">this</span>.Columns;&nbsp;c&#43;&#43;)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;id&nbsp;=&nbsp;<span class="js__string">&quot;c&quot;</span>&nbsp;&#43;&nbsp;c&nbsp;&#43;&nbsp;<span class="js__string">&quot;r&quot;</span>&nbsp;&#43;&nbsp;r;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;cell&nbsp;=&nbsp;<span class="js__operator">this</span>.cells[id];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cell.draw(g);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.cells.length=<span class="js__num">0</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span></pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p>&nbsp;</p>
<h2 style="text-align:center">Interface Code</h2>
<p>Finally we add our interface code</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>JavaScript</span><span>Visual Basic</span><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">js</span><span class="hidden">vb</span><span class="hidden">csharp</span>
<pre class="hidden">    function rand(min, max)
    {
        return Math.floor(Math.random() * (max - min &#43; 1)) &#43; min;
    }
    class Rectangle
    {
        constructor(location, size)
        {
            this.x = location.x;
            this.y = location.y
            this.location = location;
            this.width = size.width;
            this.height = size.height;
            this.right = this.x &#43; size.width;
            this.left = this.x;
            this.top = this.y;
            this.bottom = this.y &#43; this.height;
            this.size = size;
        }
    }
    class Point
    {
        constructor(x, y)
        {
            this.x = x;
            this.y = y;
        }
    }
    class Size
    {
        constructor(width, height)
        {
            this.width = width;
            this.height = height;
        }
    }
    function generate()
    {
        //This function is called by a button click event
        var cols = undefined;
        var rows = undefined;        
        var width = undefined;        
        var height = undefined;        
        var maze = undefined; 
        cols = parseInt(document.getElementById('input-cols').value);
        rows = parseInt(document.getElementById('input-rows').value);
        width = parseInt(document.getElementById('input-width').value);
        height = parseInt(document.getElementById('input-height').value);
        maze = new Maze(rows, cols, width, height);
        maze.Generate();
        delete maze;
        delete height;
        delete width;
        delete rows;
        delete cols;
    }</pre>
<pre class="hidden">Option Strict On
Option Explicit On
Option Infer Off
Public Class Form1
    Sub doLayout()
        Panel2.Top = 100
        Panel2.Left = 0
        Panel2.Height = Me.ClientRectangle.Height - Panel2.Top
        Panel2.Width = Me.ClientRectangle.Width
        Panel2.BorderStyle = BorderStyle.FixedSingle
    End Sub
    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        doLayout()
    End Sub
    Private Sub Form1_Resize(sender As Object, e As EventArgs) Handles Me.Resize
        doLayout()
    End Sub
    Private Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click
        Dim maze As New Maze(CInt(nudRows.Value), CInt(nudCols.Value), CInt(nudWidth.Value), CInt(nudHeight.Value))
        AddHandler maze.MazeComplete, Sub(m As Image)
                                          Panel1.BackgroundImage = m
                                          Panel1.BackgroundImageLayout = ImageLayout.None
                                          Panel1.Width = m.Width
                                          Panel1.Height = m.Height
                                          'maze.PrintMaze()
                                      End Sub
        maze.Generate()

    End Sub
End Class</pre>
<pre class="hidden">using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using Microsoft.VisualBasic;
using System.Collections;
using System.Diagnostics;
namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            doLayout();
        }
        public void doLayout()
        {
            Panel2.Top = 100;
            Panel2.Left = 0;
            Panel2.Height = this.ClientRectangle.Height - Panel2.Top;
            Panel2.Width = this.ClientRectangle.Width;
            Panel2.BorderStyle = BorderStyle.FixedSingle;
        }
        private void Button1_Click(object sender, EventArgs e)
        {
            Maze maze = new Maze(Convert.ToInt32(nudRows.Value), Convert.ToInt32(nudCols.Value), Convert.ToInt32(nudWidth.Value), Convert.ToInt32(nudHeight.Value));
            maze.MazeComplete &#43;= (Image m) =&gt;{
                Panel1.BackgroundImage = m;
                Panel1.BackgroundImageLayout = ImageLayout.None;
                Panel1.Width = m.Width;
                Panel1.Height = m.Height;
                //maze.PrintMaze()
            };
            maze.Generate();
        }

        private void Form1_Resize(object sender, EventArgs e)
        {
            doLayout();
        }
    }
}</pre>
<div class="preview">
<pre class="js">&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">function</span>&nbsp;rand(min,&nbsp;max)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">return</span>&nbsp;<span class="js__object">Math</span>.floor(<span class="js__object">Math</span>.random()&nbsp;*&nbsp;(max&nbsp;-&nbsp;min&nbsp;&#43;&nbsp;<span class="js__num">1</span>))&nbsp;&#43;&nbsp;min;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Rectangle&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(location,&nbsp;size)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.x&nbsp;=&nbsp;location.x;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.y&nbsp;=&nbsp;location.y&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.location&nbsp;=&nbsp;location;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.width&nbsp;=&nbsp;size.width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.height&nbsp;=&nbsp;size.height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.right&nbsp;=&nbsp;<span class="js__operator">this</span>.x&nbsp;&#43;&nbsp;size.width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.left&nbsp;=&nbsp;<span class="js__operator">this</span>.x;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.top&nbsp;=&nbsp;<span class="js__operator">this</span>.y;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.bottom&nbsp;=&nbsp;<span class="js__operator">this</span>.y&nbsp;&#43;&nbsp;<span class="js__operator">this</span>.height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.size&nbsp;=&nbsp;size;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Point&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(x,&nbsp;y)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.x&nbsp;=&nbsp;x;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.y&nbsp;=&nbsp;y;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Size&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(width,&nbsp;height)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.width&nbsp;=&nbsp;width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">this</span>.height&nbsp;=&nbsp;height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">function</span>&nbsp;generate()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">{</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__sl_comment">//This&nbsp;function&nbsp;is&nbsp;called&nbsp;by&nbsp;a&nbsp;button&nbsp;click&nbsp;event</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;cols&nbsp;=&nbsp;<span class="js__property">undefined</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;rows&nbsp;=&nbsp;<span class="js__property">undefined</span>;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;width&nbsp;=&nbsp;<span class="js__property">undefined</span>;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;height&nbsp;=&nbsp;<span class="js__property">undefined</span>;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__statement">var</span>&nbsp;maze&nbsp;=&nbsp;<span class="js__property">undefined</span>;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cols&nbsp;=&nbsp;<span class="js__function">parseInt</span>(document.getElementById(<span class="js__string">'input-cols'</span>).value);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rows&nbsp;=&nbsp;<span class="js__function">parseInt</span>(document.getElementById(<span class="js__string">'input-rows'</span>).value);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;width&nbsp;=&nbsp;<span class="js__function">parseInt</span>(document.getElementById(<span class="js__string">'input-width'</span>).value);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;height&nbsp;=&nbsp;<span class="js__function">parseInt</span>(document.getElementById(<span class="js__string">'input-height'</span>).value);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;maze&nbsp;=&nbsp;<span class="js__operator">new</span>&nbsp;Maze(rows,&nbsp;cols,&nbsp;width,&nbsp;height);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;maze.Generate();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">delete</span>&nbsp;maze;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">delete</span>&nbsp;height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">delete</span>&nbsp;width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">delete</span>&nbsp;rows;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__operator">delete</span>&nbsp;cols;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="js__brace">}</span></pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<h2 style="text-align:center">Full HTML Code</h2>
<p style="text-align:center">This code is to implement this fully in html with Javascript</p>
<p></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>HTML</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">html</span>
<pre class="hidden">&lt;div class=&quot;container&quot;&gt;
    &lt;table&gt;
        &lt;tr&gt;
            &lt;td colspan=&quot;2&quot; style=&quot;text-align:center;&quot;&gt;
                &lt;h1 style=&quot;color:white;&quot;&gt;Maze Generator&lt;/h1&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td style=&quot;text-align:center;color:white;&quot;&gt;Columns&lt;/td&gt;
            &lt;td style=&quot;text-align:center;color:white;&quot;&gt;Rows&lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td style=&quot;text-align:center;color:white;&quot;&gt;
                &lt;input type=&quot;number&quot; min=&quot;3&quot; max=&quot;50&quot; value=&quot;20&quot; id=&quot;input-cols&quot;/&gt;
            &lt;/td&gt;
            &lt;td style=&quot;text-align:center;color:white;&quot;&gt;
                &lt;input type=&quot;number&quot; min=&quot;3&quot; max=&quot;50&quot; value=&quot;20&quot; id=&quot;input-rows&quot;/&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td style=&quot;text-align:center;color:white;&quot;&gt;Cell Width(Px)&lt;/td&gt;
            &lt;td style=&quot;text-align:center;color:white;&quot;&gt;Cell Height(Px)&lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td style=&quot;text-align:center;color:white;&quot;&gt;
                &lt;input type=&quot;number&quot; min=&quot;3&quot; max=&quot;50&quot; value=&quot;20&quot; id=&quot;input-width&quot;/&gt;
            &lt;/td&gt;
            &lt;td style=&quot;text-align:center;color:white;&quot;&gt;
                &lt;input type=&quot;number&quot; min=&quot;3&quot; max=&quot;50&quot; value=&quot;20&quot; id=&quot;input-height&quot;/&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td colspan=&quot;2&quot; style=&quot;text-align:center;&quot;&gt;
                &lt;button onclick=&quot;generate()&quot;&gt;Generate Maze&lt;/button&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td colspan=&quot;2&quot; style=&quot;text-align:center;&quot;&gt;
                &lt;canvas id=&quot;MyCanvas&quot; width=&quot;300&quot; height=&quot;300&quot; style=&quot;background-color:white;&quot;&gt;&lt;/canvas&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
    &lt;/table&gt;
    &lt;br&gt;
    &lt;br&gt;
    &lt;br&gt;
&lt;/div&gt;
&lt;script&gt;
    function rand(min, max)
    {
        return Math.floor(Math.random() * (max - min &#43; 1)) &#43; min;
    }
    class Rectangle
    {
        constructor(location, size)
        {
            this.x = location.x;
            this.y = location.y
            this.location = location;
            this.width = size.width;
            this.height = size.height;
            this.right = this.x &#43; size.width;
            this.left = this.x;
            this.top = this.y;
            this.bottom = this.y &#43; this.height;
            this.size = size;
        }
    }
    class Point
    {
        constructor(x, y)
        {
            this.x = x;
            this.y = y;
        }
    }
    class Size
    {
        constructor(width, height)
        {
            this.width = width;
            this.height = height;
        }
    }
    function generate()
    {
        //This function is called by a button click event
        var cols = undefined;
        var rows = undefined;        
        var width = undefined;        
        var height = undefined;        
        var maze = undefined; 
        cols = parseInt(document.getElementById('input-cols').value);
        rows = parseInt(document.getElementById('input-rows').value);
        width = parseInt(document.getElementById('input-width').value);
        height = parseInt(document.getElementById('input-height').value);
        maze = new Maze(rows, cols, width, height);
        maze.Generate();
        delete maze;
        delete height;
        delete width;
        delete rows;
        delete cols;
    }
    class Cell
    {
        constructor(location, size, celllist, r, c, maxR, maxC)
        {
            this.Bounds = new Rectangle(location, size);
            this.Column = c;
            this.Row = r;
            this.id = &quot;c&quot; &#43; c.toString() &#43; &quot;r&quot; &#43; r.toString();
            var rowNort = r - 1;
            var rowSout = r &#43; 1;
            var colEast = c &#43; 1;
            var colWest = c - 1;
            this.NeighborNorthID = &quot;c&quot; &#43; c.toString() &#43; &quot;r&quot; &#43; rowNort.toString();
            this.NeighborSouthID = &quot;c&quot; &#43; c.toString() &#43; &quot;r&quot; &#43; rowSout.toString();
            this.NeighborEastID = &quot;c&quot; &#43; colEast.toString() &#43; &quot;r&quot; &#43; r.toString();
            this.NeighborWestID = &quot;c&quot; &#43; colWest.toString() &#43; &quot;r&quot; &#43; r.toString();
            if(rowNort &lt; 0){this.NeighborNorthID = undefined;}
            if(rowSout &gt; maxR){this.NeighborSouthID = undefined;}
            if(colEast &gt; maxC){this.NeighborEastID = undefined;}
            if(colWest &lt; 0){this.NeighborWestID = undefined;}
            this.Cells = celllist;
            this.Cells[this.id] = this;
            this.visited = false;
            this.Stack = [];
            this.NorthWall = true;
            this.SouthWall = true;
            this.WestWall = true;
            this.EastWall = true;
            this.drawLine =  function(g,x1,y1,x2,y2)
            {
                g.beginPath();
                g.strokeStyle=&quot;black&quot;;
                g.moveTo(x1 &#43; .5, y1 &#43; .5);
                g.lineTo(x2 &#43; .5, y2 &#43; .5);
                g.stroke();
            };
            this.draw = function(g)
            {
                if(this.NorthWall){this.drawLine(g,this.Bounds.left ,this.Bounds.top   ,this.Bounds.right,this.Bounds.top);}
                if(this.SouthWall){this.drawLine(g,this.Bounds.left ,this.Bounds.bottom,this.Bounds.right,this.Bounds.bottom);}
                if(this.EastWall) {this.drawLine(g,this.Bounds.right,this.Bounds.top   ,this.Bounds.right,this.Bounds.bottom);}
                if(this.WestWall) {this.drawLine(g,this.Bounds.left ,this.Bounds.top   ,this.Bounds.left ,this.Bounds.bottom);}
            };
            this.getNeighbor = function()
            {
                var unvisitedNeighbors = [];
                var northCell = this.Cells[this.NeighborNorthID];
                var southCell = this.Cells[this.NeighborSouthID];
                var eastCell = this.Cells[this.NeighborEastID];
                var westCell = this.Cells[this.NeighborWestID];
                if(northCell!= undefined){if(northCell.visited==false){unvisitedNeighbors.push(northCell);}}
                if(southCell!= undefined){if(southCell.visited==false){unvisitedNeighbors.push(southCell);}}
                if(eastCell != undefined){if(eastCell.visited ==false){unvisitedNeighbors.push(eastCell);}}
                if(westCell != undefined){if(westCell.visited ==false){unvisitedNeighbors.push(westCell);}}
                var max = unvisitedNeighbors.length-1;
                var currentCell = undefined;
                if(unvisitedNeighbors.length &gt; 0)
                {
                    var index = rand(0, max);
                    currentCell = unvisitedNeighbors[index];
                }
                return currentCell;
            };
            this.Dig = function(Stack)
            {
                this.Stack = Stack;
                var nextCell = this.getNeighbor();
                if(nextCell != undefined)
                {
                    this.Stack.push(nextCell);
                    switch(true)
                    {
                        case(nextCell.id == this.NeighborNorthID):
                            this.NorthWall = false;
                            nextCell.SouthWall = false;
                            break;
                        case(nextCell.id == this.NeighborSouthID):
                            this.SouthWall = false;
                            nextCell.NorthWall = false;
                            break;
                        case(nextCell.id == this.NeighborEastID):
                            this.EastWall = false;
                            nextCell.WestWall = false;
                            break;
                        case(nextCell.id == this.NeighborWestID):
                            this.WestWall = false;
                            nextCell.EastWall = false;
                            break;
                    }
                }
                else if(this.Stack.length != 0)
                {
                    nextCell = this.Stack.pop();
                }
                return nextCell;
            };
        }
    }
    class Maze
    {
        constructor(rows, columns, cellWidth, cellHeight)
        {
            this.Rows = rows;
            this.Columns = columns;
            this.cellWidth = cellWidth;
            this.cellHeight = cellHeight;
            this.Width = (this.Columns * this.cellWidth) &#43; 1;
            this.Height = (this.Rows * this.cellHeight) &#43; 1;
            this.cells = [];
            this.Stack = [];
            this.bounds = function()
            {
                var rect = new Rectangle(new Point(0,0), new Size(this.Width, this.Height))
                return rect;
            };
            this.Generate = function()
            {
                var c = 0;
                var r = 0;
                for(var y=0;y&lt;=this.Height;y&#43;=this.cellHeight)
                {
                    for(var x=0;x&lt;=this.Width;x&#43;=this.cellWidth)
                    {
                        var cell = new Cell(new Point(x, y), new Size(this.cellWidth, this.cellHeight), this.cells, r, c, (this.Rows - 1), (this.Columns - 1))
                       
                        c&#43;&#43;;
                    }
                    c=0;r&#43;&#43;;
                }
                this.Dig();
            };
            this.Dig = function()
            {
                var r = 0;
                var c = 0;
                var key = &quot;c0r0&quot;;
                var startCell = this.cells[key];
                this.Stack = [];
                startCell.visited = true;
                var visitedCells = 0;
                var counter= 0;
                while(startCell != undefined)
                {
                    startCell = startCell.Dig(this.Stack);
                    if(startCell != undefined)
                    {
                        startCell.visited = true;
                    }
                    counter&#43;&#43;;
                }
                this.Stack = [];
                var c = document.getElementById('MyCanvas');
                c.width = this.Width;
                c.height = this.Height;
                var g = c.getContext(&quot;2d&quot;);
                g.fillStyle=&quot;aqua&quot;;
                g.fillRect(0, 0, this.Width, this.Height);
                var size = Object.keys(this.cells).length;
                if(size &gt; 0)
                {
                    for(r = 0; r &lt;= this.Rows; r&#43;&#43;)
                    {
                        for(c = 0; c &lt;= this.Columns; c&#43;&#43;)
                        {
                            var id = &quot;c&quot; &#43; c &#43; &quot;r&quot; &#43; r;
                            var cell = this.cells[id];
                            cell.draw(g);
                        }
                    }
                }
                this.cells.length=0;
            };
        }
    }

&lt;/script&gt;
&lt;br&gt;
&lt;br&gt;
&lt;br&gt;
&lt;br&gt;
&lt;br&gt;
&lt;br&gt;
&lt;br&gt;
&lt;br&gt;
&lt;br&gt;
&lt;br&gt;</pre>
<div class="preview">
<pre class="html"><span class="html__tag_start">&lt;div</span>&nbsp;<span class="html__attr_name">class</span>=<span class="html__attr_value">&quot;container&quot;</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;table</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;tr</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">colspan</span>=<span class="html__attr_value">&quot;2&quot;</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;&quot;</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;h1</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;color:white;&quot;</span><span class="html__tag_start">&gt;</span>Maze&nbsp;Generator<span class="html__tag_end">&lt;/h1&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/tr&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;tr</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;color:white;&quot;</span><span class="html__tag_start">&gt;</span>Columns<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;color:white;&quot;</span><span class="html__tag_start">&gt;</span>Rows<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/tr&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;tr</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;color:white;&quot;</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;input</span>&nbsp;<span class="html__attr_name">type</span>=<span class="html__attr_value">&quot;number&quot;</span>&nbsp;<span class="html__attr_name">min</span>=<span class="html__attr_value">&quot;3&quot;</span>&nbsp;<span class="html__attr_name">max</span>=<span class="html__attr_value">&quot;50&quot;</span>&nbsp;<span class="html__attr_name">value</span>=<span class="html__attr_value">&quot;20&quot;</span>&nbsp;<span class="html__attr_name">id</span>=<span class="html__attr_value">&quot;input-cols&quot;</span><span class="html__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;color:white;&quot;</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;input</span>&nbsp;<span class="html__attr_name">type</span>=<span class="html__attr_value">&quot;number&quot;</span>&nbsp;<span class="html__attr_name">min</span>=<span class="html__attr_value">&quot;3&quot;</span>&nbsp;<span class="html__attr_name">max</span>=<span class="html__attr_value">&quot;50&quot;</span>&nbsp;<span class="html__attr_name">value</span>=<span class="html__attr_value">&quot;20&quot;</span>&nbsp;<span class="html__attr_name">id</span>=<span class="html__attr_value">&quot;input-rows&quot;</span><span class="html__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/tr&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;tr</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;color:white;&quot;</span><span class="html__tag_start">&gt;</span>Cell&nbsp;Width(Px)<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;color:white;&quot;</span><span class="html__tag_start">&gt;</span>Cell&nbsp;Height(Px)<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/tr&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;tr</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;color:white;&quot;</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;input</span>&nbsp;<span class="html__attr_name">type</span>=<span class="html__attr_value">&quot;number&quot;</span>&nbsp;<span class="html__attr_name">min</span>=<span class="html__attr_value">&quot;3&quot;</span>&nbsp;<span class="html__attr_name">max</span>=<span class="html__attr_value">&quot;50&quot;</span>&nbsp;<span class="html__attr_name">value</span>=<span class="html__attr_value">&quot;20&quot;</span>&nbsp;<span class="html__attr_name">id</span>=<span class="html__attr_value">&quot;input-width&quot;</span><span class="html__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;color:white;&quot;</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;input</span>&nbsp;<span class="html__attr_name">type</span>=<span class="html__attr_value">&quot;number&quot;</span>&nbsp;<span class="html__attr_name">min</span>=<span class="html__attr_value">&quot;3&quot;</span>&nbsp;<span class="html__attr_name">max</span>=<span class="html__attr_value">&quot;50&quot;</span>&nbsp;<span class="html__attr_name">value</span>=<span class="html__attr_value">&quot;20&quot;</span>&nbsp;<span class="html__attr_name">id</span>=<span class="html__attr_value">&quot;input-height&quot;</span><span class="html__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/tr&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;tr</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">colspan</span>=<span class="html__attr_value">&quot;2&quot;</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;&quot;</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;button</span>&nbsp;<span class="html__attr_name">onclick</span>=<span class="html__attr_value">&quot;generate()&quot;</span><span class="html__tag_start">&gt;</span>Generate&nbsp;Maze<span class="html__tag_end">&lt;/button&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/tr&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;tr</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;td</span>&nbsp;<span class="html__attr_name">colspan</span>=<span class="html__attr_value">&quot;2&quot;</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;text-align:center;&quot;</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;canvas</span>&nbsp;<span class="html__attr_name">id</span>=<span class="html__attr_value">&quot;MyCanvas&quot;</span>&nbsp;<span class="html__attr_name">width</span>=<span class="html__attr_value">&quot;300&quot;</span>&nbsp;<span class="html__attr_name">height</span>=<span class="html__attr_value">&quot;300&quot;</span>&nbsp;<span class="html__attr_name">style</span>=<span class="html__attr_value">&quot;background-color:white;&quot;</span><span class="html__tag_start">&gt;</span><span class="html__tag_end">&lt;/canvas&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/td&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/tr&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_end">&lt;/table&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;<span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_end">&lt;/div&gt;</span>&nbsp;
<span class="html__tag_start">&lt;script</span><span class="html__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;function&nbsp;rand(min,&nbsp;max)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;Math.floor(Math.random()&nbsp;*&nbsp;(max&nbsp;-&nbsp;min&nbsp;&#43;&nbsp;1))&nbsp;&#43;&nbsp;min;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Rectangle&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(location,&nbsp;size)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.x&nbsp;=&nbsp;location.x;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.y&nbsp;=&nbsp;location.y&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.location&nbsp;=&nbsp;location;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.width&nbsp;=&nbsp;size.width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.height&nbsp;=&nbsp;size.height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.right&nbsp;=&nbsp;this.x&nbsp;&#43;&nbsp;size.width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.left&nbsp;=&nbsp;this.x;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.top&nbsp;=&nbsp;this.y;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.bottom&nbsp;=&nbsp;this.y&nbsp;&#43;&nbsp;this.height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.size&nbsp;=&nbsp;size;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Point&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(x,&nbsp;y)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.x&nbsp;=&nbsp;x;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.y&nbsp;=&nbsp;y;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Size&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(width,&nbsp;height)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.width&nbsp;=&nbsp;width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.height&nbsp;=&nbsp;height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;function&nbsp;generate()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//This&nbsp;function&nbsp;is&nbsp;called&nbsp;by&nbsp;a&nbsp;button&nbsp;click&nbsp;event&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;cols&nbsp;=&nbsp;undefined;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;rows&nbsp;=&nbsp;undefined;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;width&nbsp;=&nbsp;undefined;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;height&nbsp;=&nbsp;undefined;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;maze&nbsp;=&nbsp;undefined;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cols&nbsp;=&nbsp;parseInt(document.getElementById('input-cols').value);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rows&nbsp;=&nbsp;parseInt(document.getElementById('input-rows').value);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;width&nbsp;=&nbsp;parseInt(document.getElementById('input-width').value);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;height&nbsp;=&nbsp;parseInt(document.getElementById('input-height').value);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;maze&nbsp;=&nbsp;new&nbsp;Maze(rows,&nbsp;cols,&nbsp;width,&nbsp;height);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;maze.Generate();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;delete&nbsp;maze;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;delete&nbsp;height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;delete&nbsp;width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;delete&nbsp;rows;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;delete&nbsp;cols;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Cell&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(location,&nbsp;size,&nbsp;celllist,&nbsp;r,&nbsp;c,&nbsp;maxR,&nbsp;maxC)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Bounds&nbsp;=&nbsp;new&nbsp;Rectangle(location,&nbsp;size);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Column&nbsp;=&nbsp;c;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Row&nbsp;=&nbsp;r;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.id&nbsp;=&nbsp;&quot;c&quot;&nbsp;&#43;&nbsp;c.toString()&nbsp;&#43;&nbsp;&quot;r&quot;&nbsp;&#43;&nbsp;r.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;rowNort&nbsp;=&nbsp;r&nbsp;-&nbsp;1;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;rowSout&nbsp;=&nbsp;r&nbsp;&#43;&nbsp;1;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;colEast&nbsp;=&nbsp;c&nbsp;&#43;&nbsp;1;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;colWest&nbsp;=&nbsp;c&nbsp;-&nbsp;1;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.NeighborNorthID&nbsp;=&nbsp;&quot;c&quot;&nbsp;&#43;&nbsp;c.toString()&nbsp;&#43;&nbsp;&quot;r&quot;&nbsp;&#43;&nbsp;rowNort.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.NeighborSouthID&nbsp;=&nbsp;&quot;c&quot;&nbsp;&#43;&nbsp;c.toString()&nbsp;&#43;&nbsp;&quot;r&quot;&nbsp;&#43;&nbsp;rowSout.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.NeighborEastID&nbsp;=&nbsp;&quot;c&quot;&nbsp;&#43;&nbsp;colEast.toString()&nbsp;&#43;&nbsp;&quot;r&quot;&nbsp;&#43;&nbsp;r.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.NeighborWestID&nbsp;=&nbsp;&quot;c&quot;&nbsp;&#43;&nbsp;colWest.toString()&nbsp;&#43;&nbsp;&quot;r&quot;&nbsp;&#43;&nbsp;r.toString();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(rowNort&nbsp;&lt;&nbsp;0){this.NeighborNorthID&nbsp;=&nbsp;undefined;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(rowSout&nbsp;&gt;&nbsp;maxR){this.NeighborSouthID&nbsp;=&nbsp;undefined;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(colEast&nbsp;&gt;&nbsp;maxC){this.NeighborEastID&nbsp;=&nbsp;undefined;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(colWest&nbsp;&lt;&nbsp;0){this.NeighborWestID&nbsp;=&nbsp;undefined;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Cells&nbsp;=&nbsp;celllist;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Cells[this.id]&nbsp;=&nbsp;this;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.visited&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Stack&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.NorthWall&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.SouthWall&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.WestWall&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.EastWall&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.drawLine&nbsp;=&nbsp;&nbsp;function(g,x1,y1,x2,y2)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.beginPath();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.strokeStyle=&quot;black&quot;;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.moveTo(x1&nbsp;&#43;&nbsp;.5,&nbsp;y1&nbsp;&#43;&nbsp;.5);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.lineTo(x2&nbsp;&#43;&nbsp;.5,&nbsp;y2&nbsp;&#43;&nbsp;.5);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.stroke();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.draw&nbsp;=&nbsp;function(g)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(this.NorthWall){this.drawLine(g,this.Bounds.left&nbsp;,this.Bounds.top&nbsp;&nbsp;&nbsp;,this.Bounds.right,this.Bounds.top);}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(this.SouthWall){this.drawLine(g,this.Bounds.left&nbsp;,this.Bounds.bottom,this.Bounds.right,this.Bounds.bottom);}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(this.EastWall)&nbsp;{this.drawLine(g,this.Bounds.right,this.Bounds.top&nbsp;&nbsp;&nbsp;,this.Bounds.right,this.Bounds.bottom);}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(this.WestWall)&nbsp;{this.drawLine(g,this.Bounds.left&nbsp;,this.Bounds.top&nbsp;&nbsp;&nbsp;,this.Bounds.left&nbsp;,this.Bounds.bottom);}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.getNeighbor&nbsp;=&nbsp;function()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;unvisitedNeighbors&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;northCell&nbsp;=&nbsp;this.Cells[this.NeighborNorthID];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;southCell&nbsp;=&nbsp;this.Cells[this.NeighborSouthID];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;eastCell&nbsp;=&nbsp;this.Cells[this.NeighborEastID];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;westCell&nbsp;=&nbsp;this.Cells[this.NeighborWestID];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(northCell!=&nbsp;undefined){if(northCell.visited==false){unvisitedNeighbors.push(northCell);}}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(southCell!=&nbsp;undefined){if(southCell.visited==false){unvisitedNeighbors.push(southCell);}}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(eastCell&nbsp;!=&nbsp;undefined){if(eastCell.visited&nbsp;==false){unvisitedNeighbors.push(eastCell);}}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(westCell&nbsp;!=&nbsp;undefined){if(westCell.visited&nbsp;==false){unvisitedNeighbors.push(westCell);}}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;max&nbsp;=&nbsp;unvisitedNeighbors.length-1;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;currentCell&nbsp;=&nbsp;undefined;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(unvisitedNeighbors.length&nbsp;&gt;&nbsp;0)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;index&nbsp;=&nbsp;rand(0,&nbsp;max);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;currentCell&nbsp;=&nbsp;unvisitedNeighbors[index];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;currentCell;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Dig&nbsp;=&nbsp;function(Stack)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Stack&nbsp;=&nbsp;Stack;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;nextCell&nbsp;=&nbsp;this.getNeighbor();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(nextCell&nbsp;!=&nbsp;undefined)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Stack.push(nextCell);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;switch(true)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case(nextCell.id&nbsp;==&nbsp;this.NeighborNorthID):&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.NorthWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell.SouthWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;break;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case(nextCell.id&nbsp;==&nbsp;this.NeighborSouthID):&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.SouthWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell.NorthWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;break;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case(nextCell.id&nbsp;==&nbsp;this.NeighborEastID):&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.EastWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell.WestWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;break;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case(nextCell.id&nbsp;==&nbsp;this.NeighborWestID):&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.WestWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell.EastWall&nbsp;=&nbsp;false;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;break;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;else&nbsp;if(this.Stack.length&nbsp;!=&nbsp;0)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nextCell&nbsp;=&nbsp;this.Stack.pop();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;nextCell;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;Maze&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constructor(rows,&nbsp;columns,&nbsp;cellWidth,&nbsp;cellHeight)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Rows&nbsp;=&nbsp;rows;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Columns&nbsp;=&nbsp;columns;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.cellWidth&nbsp;=&nbsp;cellWidth;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.cellHeight&nbsp;=&nbsp;cellHeight;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Width&nbsp;=&nbsp;(this.Columns&nbsp;*&nbsp;this.cellWidth)&nbsp;&#43;&nbsp;1;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Height&nbsp;=&nbsp;(this.Rows&nbsp;*&nbsp;this.cellHeight)&nbsp;&#43;&nbsp;1;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.cells&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Stack&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.bounds&nbsp;=&nbsp;function()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;rect&nbsp;=&nbsp;new&nbsp;Rectangle(new&nbsp;Point(0,0),&nbsp;new&nbsp;Size(this.Width,&nbsp;this.Height))&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;rect;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Generate&nbsp;=&nbsp;function()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;c&nbsp;=&nbsp;0;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;r&nbsp;=&nbsp;0;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for(var&nbsp;y=0;y&lt;=this.Height;y&#43;=this.cellHeight)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for(var&nbsp;x=0;x&lt;=this.Width;x&#43;=this.cellWidth)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;cell&nbsp;=&nbsp;new&nbsp;Cell(new&nbsp;Point(x,&nbsp;y),&nbsp;new&nbsp;Size(this.cellWidth,&nbsp;this.cellHeight),&nbsp;this.cells,&nbsp;r,&nbsp;c,&nbsp;(this.Rows&nbsp;-&nbsp;1),&nbsp;(this.Columns&nbsp;-&nbsp;1))&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c&#43;&#43;;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c=0;r&#43;&#43;;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Dig();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Dig&nbsp;=&nbsp;function()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;r&nbsp;=&nbsp;0;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;c&nbsp;=&nbsp;0;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;key&nbsp;=&nbsp;&quot;c0r0&quot;;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;startCell&nbsp;=&nbsp;this.cells[key];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Stack&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;startCell.visited&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;visitedCells&nbsp;=&nbsp;0;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;counter=&nbsp;0;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;while(startCell&nbsp;!=&nbsp;undefined)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;startCell&nbsp;=&nbsp;startCell.Dig(this.Stack);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(startCell&nbsp;!=&nbsp;undefined)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;startCell.visited&nbsp;=&nbsp;true;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;counter&#43;&#43;;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.Stack&nbsp;=&nbsp;[];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;c&nbsp;=&nbsp;document.getElementById('MyCanvas');&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c.width&nbsp;=&nbsp;this.Width;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c.height&nbsp;=&nbsp;this.Height;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;g&nbsp;=&nbsp;c.getContext(&quot;2d&quot;);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.fillStyle=&quot;aqua&quot;;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;g.fillRect(0,&nbsp;0,&nbsp;this.Width,&nbsp;this.Height);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;size&nbsp;=&nbsp;Object.keys(this.cells).length;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(size&nbsp;&gt;&nbsp;0)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for(r&nbsp;=&nbsp;0;&nbsp;r&nbsp;&lt;=&nbsp;this.Rows;&nbsp;r&#43;&#43;)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for(c&nbsp;=&nbsp;0;&nbsp;c&nbsp;&lt;=&nbsp;this.Columns;&nbsp;c&#43;&#43;)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;id&nbsp;=&nbsp;&quot;c&quot;&nbsp;&#43;&nbsp;c&nbsp;&#43;&nbsp;&quot;r&quot;&nbsp;&#43;&nbsp;r;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var&nbsp;cell&nbsp;=&nbsp;this.cells[id];&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cell.draw(g);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.cells.length=0;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;
<span class="html__tag_end">&lt;/script&gt;</span>&nbsp;
<span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;&nbsp;
</span><span class="html__tag_start">&lt;br</span><span class="html__tag_start">&gt;</span></pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p></p>