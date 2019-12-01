# Lunar-lander
I have used breakpoints this code is broken. I am still learning didnt know if anyone could help.
Public Class Form1
    Inherits System.Windows.Forms.Form
    Dim g As System.Drawing.Graphics
    Dim SpaceshipX As Integer
    Dim SpaceshipY As Integer ' Location of feet of Spaceship
    Dim VelocityX As Double
    Dim VelocityY As Double
    Dim GroundY As Integer
    Dim Clock As System.Timers.Timer
    Dim myBitmap1 As System.Drawing.Bitmap ' Not firing spaceship
    Dim myBitmap2 As System.Drawing.Bitmap ' Firing Spaceship
    Dim myBitmap3 As System.Drawing.Bitmap ' Moon
    Dim Fuel As Integer


#Region " Windows Form Designer generated code "

    Public Sub New()
        MyBase.New()

        'This call is required by the Windows Form Designer.
        InitializeComponent()

        'Add any initialization after the InitializeComponent() call

    End Sub

    'Form overrides dispose to clean up the component list.
    Protected Overloads Overrides Sub Dispose(ByVal disposing As Boolean)
        If disposing Then
            If Not (components Is Nothing) Then
                components.Dispose()
            End If
        End If
        MyBase.Dispose(disposing)
    End Sub

    'Required by the Windows Form Designer
    Private components As System.ComponentModel.IContainer

    'NOTE: The following procedure is required by the Windows Form Designer
    'It can be modified using the Windows Form Designer.  
    'Do not modify it using the code editor.
    Friend WithEvents btnStart As System.Windows.Forms.Button
    Friend WithEvents btnSetup As System.Windows.Forms.Button
    Friend WithEvents btnThrusters As System.Windows.Forms.Button
    Friend WithEvents btnLeft As System.Windows.Forms.Button
    Friend WithEvents btnRight As System.Windows.Forms.Button
    <System.Diagnostics.DebuggerStepThrough()> Private Sub InitializeComponent()
        Me.btnStart = New System.Windows.Forms.Button()
        Me.btnSetup = New System.Windows.Forms.Button()
        Me.btnThrusters = New System.Windows.Forms.Button()
        Me.btnLeft = New System.Windows.Forms.Button()
        Me.btnRight = New System.Windows.Forms.Button()
        Me.SuspendLayout()
        '
        'btnStart
        '
        Me.btnStart.Location = New System.Drawing.Point(152, 328)
        Me.btnStart.Name = "btnStart"
        Me.btnStart.Size = New System.Drawing.Size(80, 24)
        Me.btnStart.TabIndex = 0
        Me.btnStart.Text = "Start Sim"
        '
        'btnSetup
        '
        Me.btnSetup.Location = New System.Drawing.Point(40, 328)
        Me.btnSetup.Name = "btnSetup"
        Me.btnSetup.Size = New System.Drawing.Size(75, 23)
        Me.btnSetup.TabIndex = 1
        Me.btnSetup.Text = "Setup"
        '
        'btnThrusters
        '
        Me.btnThrusters.Location = New System.Drawing.Point(312, 328)
        Me.btnThrusters.Name = "btnThrusters"
        Me.btnThrusters.Size = New System.Drawing.Size(88, 23)
        Me.btnThrusters.TabIndex = 2
        Me.btnThrusters.Text = "Fire Thrusters"
        '
        'btnLeft
        '
        Me.btnLeft.Location = New System.Drawing.Point(264, 328)
        Me.btnLeft.Name = "btnLeft"
        Me.btnLeft.Size = New System.Drawing.Size(40, 23)
        Me.btnLeft.TabIndex = 3
        Me.btnLeft.Text = "Left"
        '
        'btnRight
        '
        Me.btnRight.Location = New System.Drawing.Point(408, 328)
        Me.btnRight.Name = "btnRight"
        Me.btnRight.Size = New System.Drawing.Size(40, 23)
        Me.btnRight.TabIndex = 4
        Me.btnRight.Text = "Right"
        '
        'Form1
        '
        Me.AutoScaleBaseSize = New System.Drawing.Size(5, 13)
        Me.ClientSize = New System.Drawing.Size(456, 373)
        Me.Controls.Add(Me.btnRight)
        Me.Controls.Add(Me.btnLeft)
        Me.Controls.Add(Me.btnThrusters)
        Me.Controls.Add(Me.btnSetup)
        Me.Controls.Add(Me.btnStart)
        Me.FormBorderStyle = System.Windows.Forms.FormBorderStyle.Fixed3D
        Me.Name = "Form1"
        Me.Text = "Lunar Lander Simulation"
        Me.ResumeLayout(False)

    End Sub

#End Region

    Private Sub Form1_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load
        g = Me.CreateGraphics
        myBitmap1 = New System.Drawing.Bitmap("final spaceship.bmp")
        myBitmap2 = New System.Drawing.Bitmap("final firing spaceship.bmp")
        myBitmap3 = New System.Drawing.Bitmap("Moon.bmp")
        Clock = New System.Timers.Timer
        Clock.Enabled = False
        Clock.Interval = 100
        AddHandler Clock.Elapsed, AddressOf OnTimer
        GroundY = 270
    End Sub

    Sub Setup()
        Dim b As System.Drawing.Brush
        b = New System.Drawing.SolidBrush(Color.Black)
        g.FillRectangle(b, New Rectangle(2, 2, 453, 300))
        SpaceshipX = 230
        SpaceshipY = 60
        DrawSpaceship(SpaceshipX, SpaceshipY, False)
        VelocityX = 0
        VelocityY = 0
        Fuel = 200
        Dim GroundPen As System.Drawing.Pen
        GroundPen = New System.Drawing.Pen(Color.Gray)
        g.DrawLine(GroundPen, 2, GroundY, 453, GroundY)
    End Sub

    Private Sub btnSetup_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnSetup.Click
        Setup()

        ' Display exsisting bitmap

        g.DrawImage(myBitmap3, 390, 2)

        ' Create and display runtime bitmap
        Dim flag As New System.Drawing.Bitmap(20, 13)
        Dim x As Integer
        Dim y As Integer
        ' Make flag white
        For x = 0 To flag.Width - 1
            For y = 0 To flag.Height - 1
                flag.SetPixel(x, y, Color.White)
            Next
        Next
        ' Red stripes
        For y = 0 To flag.Height - 1 Step 2
            For x = 0 To flag.Width - 1
                flag.SetPixel(x, y, Color.Red)
            Next
        Next
        ' Put in blue field
        For x = 0 To 8
            For y = 0 To 7
                flag.SetPixel(x, y, Color.Blue)
            Next
        Next
        ' Stars
        flag.SetPixel(1, 1, Color.White)
        flag.SetPixel(3, 1, Color.White)
        flag.SetPixel(5, 1, Color.White)
        flag.SetPixel(7, 1, Color.White)
        flag.SetPixel(1, 3, Color.White)
        flag.SetPixel(3, 3, Color.White)
        flag.SetPixel(5, 3, Color.White)
        flag.SetPixel(7, 3, Color.White)
        flag.SetPixel(1, 5, Color.White)
        flag.SetPixel(3, 5, Color.White)
        flag.SetPixel(5, 5, Color.White)
        flag.SetPixel(7, 5, Color.White)
        ' Draw
        g.DrawImage(flag, 10, 10)
    End Sub

    Sub DrawSpaceship(ByVal x As Integer, ByVal y As Integer, ByVal Firing As Boolean)
        'btnThrusters.Enabled = False
        If Firing = True Then
            g.DrawImage(myBitmap2, x - 20, y - 49)
        Else
            g.DrawImage(myBitmap1, x - 20, y - 49)
        End If
        'btnThrusters.Enabled = True Too much time drawing
    End Sub

    Sub EraseSpaceship(ByVal x As Integer, ByVal y As Integer)
        Dim BlackBrush As System.Drawing.Brush
        BlackBrush = New System.Drawing.SolidBrush(Color.Black)
        g.FillRectangle(BlackBrush, New Rectangle(x - 20, y - 49, 40, 49))
        BlackBrush.Dispose()
    End Sub

    Public Sub OnTimer(ByVal source As Object, ByVal e As System.Timers.ElapsedEventArgs)
        ' Calculate New Location of Spaceship
        Dim NewX As Integer
        Dim NewY As Integer
        ' Note: No change in x component of velocity
        VelocityY = VelocityY + 9.8 / (10000 / Clock.Interval)
        ' Button2.Text = VelocityY DEBUG
        ' In line above, the 10000 is simulation speed. Increase to slow down simulation.
        NewY = SpaceshipY + VelocityY
        NewX = SpaceshipX + VelocityX
        If NewY <> SpaceshipY Or NewX <> SpaceshipX Then
            ' Erase Spaceship
            EraseSpaceship(SpaceshipX, SpaceshipY)
            ' Update location of Spaceship
            SpaceshipX = NewX
            SpaceshipY = NewY
            ' Re-Draw Spaceship
            DrawSpaceship(SpaceshipX, SpaceshipY, False)
        End If
        If SpaceshipY >= GroundY Then
            Clock.Enabled = False ' Shut Down Simulation
            ' Disable thrusters
            If VelocityY <= 0.9 Then
                MsgBox("Houston We Have Landed")
            Else
                MsgBox("Houston We Have A Problem")
            End If
        End If

    End Sub

    Private Sub btnStart_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnStart.Click
        Clock.Enabled = True
    End Sub

    Private Sub btnThrusters_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnThrusters.Click
        If Fuel > 0 Then
            ' Replace non-firing image with firing image
            DrawSpaceship(SpaceshipX, SpaceshipY, True)
            ' Adjust velocity
            VelocityY = VelocityY - 0.5
            Fuel = Fuel - 5
        End If
    End Sub

    Private Sub btnLeft_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnLeft.Click
        If Fuel > 0 Then
            VelocityX = VelocityX - 0.5
            Fuel = Fuel - 5
        End If
    End Sub

    Private Sub btnRight_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnRight.Click
        If Fuel > 0 Then
            VelocityX = VelocityX + 0.5
            Fuel = Fuel - 5
        End If
    End Sub
End Class
