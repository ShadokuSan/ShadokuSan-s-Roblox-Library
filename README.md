<div align="center"><h1>ShadokuSan's Roblox Library</h1></div>

<div align="center"><h3>This is a module script that makes some common tasks, or tedious ones, easier than before.</h3></div>

#

:warning: This is not intended to be used as a learning tool for coding, and nor do I claim to be an expert in coding. So.. take the quality and readability of this module with a little grain of salt.

**Module Link:** <https://www.roblox.com/library/3165359492/redirect>

**Original DevForum Post:** <https://devforum.roblox.com/t/346953>
___

### **Usage**

Simply use **require(3165359492)** or insert the module manually to use it (but don't forget that requiring a ModuleScript via an ID doesn't work in LocalScripts).

For the sake of the examples in the functions list below, let's assume **Code** is the variable used:

```lua
local Code = require(3165359492)
```

___

<details><summary><h3>Variables List</h3></summary>

| Variable | Description |
| --- | --- |
| Script | Refers to the module's instance itself. |
| Warnings | Tied to the **Warnings** attribute to the module. This is used to give information in some scripts for potentially incorrect uses but is an instance that may be auto-corrected. |
| ManualErrors | Tied to the **ManualErrors** attribute to the module. Normally, this will insert errors in areas where incorrect usage of the module likely cannot be auto-corrected and tries to send a message that will try to make some sense of what went wrong. |
| Formulas | This is a table that hosts multiple semi-commonly used formulas, put into function form. See [Formulas](#formulas) for more information. |

### Formulas

This is a table that hosts multiple semi-commonly used formulas, put into function form. Followed by, `Code.Formulas.NameHere`

| Forumla Name | Format | Description |
| --- | --- | --- |
| GetAngleVector2 | GetAngleVector2(Position1: Vector2, Position2: Vector2) | Returns an angle where **Position1** points at **Position2**. |
| RayReflection | RayReflection (DirectionNormal: Vector3, SurfaceNormal: Vector3, Modifier: number?) | Returns a Vector3 direction/normal by taking the **DirectionNormal** and bouncing it off of the **SurfaceNormal**; angle depends on the **Modifier** which is defaulted at 2. |
| PythagoreanTheorem | PythagoreanTheorem(Number1: number, Number2: number) | Simply returns the result of √**Number1**<sup>2</sup> + **Number2**<sup>2</sup> |
| PointOnRay | PointOnRay(Point1: Vector3, Point2: Vector3, ReferencePoint: Vector3) | Returns a position by making a ray/line between **Point1** and **Point2**, then uses the **ReferencePoint** to find the closest position from said line. |
| Lerp | Lerp(Start: number, End: number, Alpha: number) | Returns the number between the **Start** and **End** based on the Alpha (between 0-1). |
| TimeConvert | TimeConvert(Seconds: number, TimeUnit: string<"Milliseconds", "Seconds", "Hours", "Days", "Weeks">) | Returns the conversion of seconds to another time unit. |

#

</details>

<details><summary><h3>Functions List</h3></summary>

<details><summary>Create</summary>

**Aliases:** new

**Description:** Customized "Instance.new" function that allows you to edit multiple properties at once.

**Setup:** `Code.new("InstanceName", Parent, ParentFirst){Properties}`

**Returns:** The new Instance that was created.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| InstanceName | string | nil | The ClassName of the instance you want to create. |
| Parent | Instance | nil | Where this instance will be parented under. Occurs after all other properties are set. |
| ParentFirst | boolean | false | Will set the parent before all other properties instead. |
| Properties | table | {} | The properties of the instance you're creating. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
--Simply make a new part that will be parented to the workspace.
local Part = Code.new('Part',workspace){Name = "TestPart",Position = Vector3.new(0,5,6),Anchored = true}

--Another part is made, but is parented under the workspace before the properties are set.
local Part = Code.new('Part',workspace,true){Name = "TestPart",Position = Vector3.new(0,5,6),Anchored = true}

--Special Input: Same | We'll make this part have the reflectance and transparency property set to 0.5
local Part = Code.new("Part",workspace){Name = "TestPart",Same={0.5,"Transparency","Reflectance"}}
```

___
</details>

<details><summary>Change</summary>

**Description:** Change multiple properties of 1 or more Instances at once.

**Setup:** `Code.Change(Instances...){Properties}`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instances | Instance / {Instance...} | nil | The instance(s) that you wish to edit. |
| Properties | table | {} | A dictionary of the properties/attributes of the instance(s) you're editing. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
--Example 1:
Code.Change(Part){Color = Color3.new(1,5,2),CanCollide = false}
--Example 2:
Code.Change(BoolValue1,BoolValue2,BoolValue3){Value = false,Parent = workspace}
--Special Inputs 1:
Code.Change(BoolValue1,BoolValue2){Value = "not"}
    --Returns their opposite values for each one.
--Special Inputs 2:
Code.Change(NumberValue1,NumberValue2){Value = "+5"}
    --Adds 5 to each individual number value.
--Special Inputs 3:
Code.Change(NumberValue1,NumberValue2){Same={0.5,"Transparency","Reflectance"}}
    --Makes their transparency and reflectance values 0.5.
--Special Inputs 4:
Code.Change(PartTable){Position = "~0,5,0"}
    --Moves each part in your PartTable 5 studs up independent of each other.
--Special Inputs 5:
Code.Change(PartTable){CFrame = "~0,5,0"}
    --Moves each part in your PartTable 5 studs up relatively via CFrame:ToWorldSpace.
Code.Change(PartTable){CFrame = "@0,90,0"}
    --Rotates each part 90 degrees on the Y-axis.
Code.Change(PartTable){CFrame = "<0,5,0,0,90,0"}
    --Moves each part in your PartTable 5 studs up relatively via CFrame:ToWorldSpace, and then applies a 90 degree rotation on the Y-Axis. Using > will do the inverse order.
--Special Inputs 6:
function Func(Part)
return Part.CFrame:ToWorldSpace(CFrame.new(0,5,0))
end

Code.Change(PartTable){CFrame = Func}
    --Moves each part in your PartTable 5 studs up independently of each other and their rotation.
```

___
</details>

<details><summary>ChangeSame</summary>

**Aliases:** Change2

**Description:** Change multiple properties of 1 or more Instances to the same value (if possible).

**Setup:** `Code.ChangeSame(Value, Instances...){Properties}`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Value | Any | nil | The value that the Properties are being changed to (if possible). |
| Instances | Instance | nil | The Instance(s) that you're editing. |
| Properties | table | {} | A dictionary of the properties/attributes of the instance(s) you're editing. |

### Usage Example

```lua
--Example 1:
Code.ChangeSame(true,Part){"Massless","CanCollide"}
--Example 2:
Code.ChangeSame(true,Part1,Part2,Union1){"Massless","CanCollide"}
```

___
</details>

<details><summary>Clone</summary>

**Aliases:** Copy

**Description:** Clone an item and edit its properties at the same time.

**Setup:** `Code.Clone(Item, SameParent, ParentFirst){Properties}`

**Returns:** The clone of the instance.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Item | Instance | nil | The Instance that you wish to clone. |
| SameParent | boolean | false | Determines if the cloned instance is parented under the same parent as the original. |
| ParentFirst | boolean | false |  Determines if the cloned instance is parented before<sub>(true)</sub> the property changes or after.<sub>(false)</sub> Only takes effect if SameParent is set to true. |
| Properties | table | {} | The properties/attributes of the instance you're cloning; if you're changing any. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
--Example 1: 
local Brick = Code.Clone(workspace.Brick,true){Name = "Cloned",Position = Vector3.new(0,10,0)}
--Example 2:
local Brick = Code.Clone(workspace.Brick){Name = "Cloned",Position = Vector3.new(0,10,0),Parent = workspace}
--Special Input: Same | We'll make this part be unanchored and noncollidable
local Brick = Code.Clone(workspace.Brick){Name = "Cloned",Same = {false,"Anchored","CanCollide"}}
```

___
</details>

<details><summary>Replace</summary>

**Description:** Replace an Instance by creating a new one or cloning another in its place.

**Setup:** `Code.Replace(Replacee, Replacement, SameParent, ParentFirst){Properties}`

**Returns:** The replacement instance.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Replacee | Instance | nil | The Instance that you wish to replace; gets destroyed in the process. |
| Replacement | Instance / string | nil | The Instance that you wish to clone or a string for the class that you want to create. |
| SameParent | boolean | false | Determines if the cloned instance is parented under the same parent as the original. |
| ParentFirst | boolean | false | Determines if the cloned instance is parented before<sub>(true)</sub> the property changes or after.<sub>(false)</sub> Only takes effect if SameParent is set to true. |
| Properties | table | {} | The properties/attributes of the instance you're using as the replacement; if you're changing any. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
--Example 1: 
local Brick = Code.Replace(workspace.Brick1,workspace.Brick2,true){Name = "Brick3"}
    --Destroys Brick1, clones Brick2 and names it Brick3. Variable Brick becomes Brick3. Brick3 gets parented to the same parent as Brick1.
--Example 2:
local Brick = Code.Replace(workspace.Brick1,"Part"){Name = "Brick2",CFrame = workspace.Brick1.CFrame}
    --Destroys Brick1, makes a new part that gets named Brick2, and makes the CFrame the same.
```

___
</details>

<details><summary>Call</summary>

**Description:** Call multiple functions on a single instance at roughly the same time.

**Setup:** `Code.Call(Instance, Functions)`

**Returns:** All of the potential returns the functions may have given.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | The Instance that the functions are being called on. |
| Functions | table | {} | A list of functions and their accompanying parameters. |

### Usage Example

```lua
local Part1,Part2 = workspace.Part1,workspace.Part2
local Result,FullName = Code.Call(Part1,{CanCollideWith = {Part2},GetFullName = true})
--Result will be a boolean telling us if the 2 parts are able to interact with each other.
--FullName will fetch the full name of said part.
```

___
</details>

<details><summary>Destroy</summary>

**Aliases:** Null, Nullify

**Description:** Destroys a bunch of Instances at once.

**Setup:** `Code.Destroy(Instances...)`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instances | Instance / {Instance...} | nil | The Instance(s) that you're deleting. |

### Usage Example

```lua
Code.Destroy(workspace.Brick1,workspace.Brick2,workspace.Brick3)
```

**Note:** Even if the item for some reason doesn't exist, it will not error and not stop the script it's used in.
___
</details>

<details><summary>Find</summary>

**Aliases:** Search

**Description:** Advanced Instance searcher.

**Setup:** `Code.Find(Instance, ReturnFirst, CheckDescendants, MaxAmount, FrameSpeed, IgnoreList){Properties}`

**Returns:**
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | Where to look. |
| ReturnFirst | boolean | false | Returns the first instance it can find that matches. |
| CheckDescendants | boolean | false | If it will search all descendants like :GetAllDescendants() |
| MaxAmount | number | :infinity: | The number of instances you want to be returned (works only if ReturnFirst is false). |
| FrameSpeed | number | 0 | A delay between each item searched. Good if you're searching a lot of items and it tends to lag. |
| IgnoreList | table | {} |  An array of Instance(s) you want ignored, including its children. |
| Properties | table | {} | A dictionary of the properties/attributes of the instance(s) you’re looking for. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Finding Values](#on-searching-values) section for details on how to use them.

### Usage Example

```lua
--Example 1:
local Get = Code.Find(workspace.Model,false,true){Name="Brick","Anchored"=true}
--Example 2:
local Get = Code.Find(workspace.Model,true){Name="Brick","Anchored"=true}
--Example 3:
local Get = Code.Find(workspace.Model,false,true,10,2){ClassName="Part"}
    --10 will return 10 items, or less if there aren't 10 that match all of the Properties.
    --1 means it'll search and add every found item every 2 frames.
--Special Inputs 1:
local Get = Code.Find(workspace.Model){Name="...Brick",Transparency="<1",Color="R"}
    --returns any parts that have names that end with "Brick",
    --has a transparency less than 1, and their Color value is dominantly red.
--Special Inputs 2:
local Get = Code.Find(workspace.Model){Name="Brick...",Reflectance=">=0.5",BrickColor="...red"}
    --returns any parts that have names that start with "Brick",
    --has their reflectance set to 0.5 or greater,
    --and their BrickColor is any BrickColor ending in "red".
--Special Inputs 3:
local Get = Code.Find(workspace.Model,false,true){IsA="BasePart"}
    --returns all instances that are classified as base parts.
--Special Inputs 4:
local Get = Code.Find(workspace.Model,false,true){Name={"Test1","Test2"}}
    --returns all parts that have their names as either Test1 or Test2.
--Special Inputs 5:
local Get = Code.Find(workspace,true,true){IsA="BasePart",IsPartOf={{"Folder",1}}}
    --returns the first BasePart it finds that is also the direct child of a Folder Instance.
    --Check the IsPartOf function of this module to see the general set-up.
--Special Inputs 6:
local Get = Code.Find(workspace.true,false){IsA="BasePart",["Position.X"]>="<10"}
    --returns all parts in the workspace with a position of X that is less than 10
--Special Inputs 7:
local Get = Code.Find(workspace.true,false){IsA="BasePart",Attribute={"Test",5}}
    --returns all parts in the workspace that has an attribute named "Test" that are also equal to 5
```

___
</details>

<details><summary>FindChange</summary>

**Description:** Combination of the Find and Change functions.

**Setup:** `Code.FindChange(Instance, ReturnFirst, CheckDescendants, MaxAmount, FrameSpeed, IgnoreList){HasProperties}{ChangeProperties}`

**Returns:** The Instances that were found and changed, or false if not.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | Where to look. |
| ReturnFirst | boolean | false | Returns the first instance it can find that matches. |
| CheckDescendants | boolean | false | If it will search all descendants like :GetAllDescendants() |
| MaxAmount | number | :infinity: | The number of instances you want to be returned (works only if ReturnFirst is false). |
| FrameSpeed | number | 0 | A delay between each item searched. Good if you're searching a lot of items and it tends to lag. |
| IgnoreList | table | {} |  An array of Instance(s) you want ignored, including its children. |
| HasProperties | table | {} | A dictionary of the properties/attributes of the instance(s) you’re looking for. |
| ChangeProperties | table | {} | A dictionary of the properties/attributes of the instance(s) you’re editing. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) and [On Finding Values](#on-searching-values) sections for details on how to use them.

### Usage Example

```lua
local Get = Code.FindChange(workspace,true,false){["Position.X"]> = "<=-0.5"}{Material=Enum.Material.Neon}
print("Got:",Get)
--Will find parts that have a position value of X that is less than or equal to -0.5, then changes all of their materials to neon. Then returns a list of the changed parts.
```

___
</details>

<details><summary>FindDestroy</summary>

**Description:** Combination of the Find and Destroy functions.

**Setup:** `Code.FindDestroy(Instance, CheckDescendants, MaxAmount, FrameSpeed, IgnoreList){HasProperties}`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | Where to look. |
| CheckDescendants | boolean | false | If it will search all descendants like :GetAllDescendants() |
| MaxAmount | number | :infinity: | The number of instances you want to be destroyed. |
| FrameSpeed | number | 0 | A delay between each item searched. Good if you're searching a lot of items and it tends to lag. |
| IgnoreList | table | {} |  An array of Instance(s) you want ignored, including its children. |
| HasProperties | table | {} | A dictionary of the properties/attributes of the instance(s) you’re looking for. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Finding Values](#on-searching-values) section for details on how to use them.

### Usage Example

```lua
Code.FindDestroy(workspace,false,5){["Position.X"]> = "<1"}
--Will find the first 5 parts that have a position value of X that is less than 1, then destroys them.
```

___
</details>

<details><summary>FindAllChildren</summary>
**Aliases:** FindAll, FAC

**Description:** Acts like Instance:FindFirstChild() where you can search for multiple instances.

**Setup:** `Code.FindAllChildren(Instance, Recursive, Items...)`

**Returns:** The Instances that were found, or false if not.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | The Instance you'd like to search in. |
| Recursive | boolean | false | Whether or not the search should be conducted recursively. |
| Items | string | nil | A bunch of strings (for names) you'd like to search for. |

### Usage Example

```lua
--Example 1:
local PartA,PartB,PartC = Code.FindAllChildren(workspace,false,"PartA","PartB","PartC")
--Will return either the Instances that has those names or false if not.
--Could look like: PartA, false, PartC if there is no PartB

--Example 2:
local Mesh,Texture = Code.FindAllChildren(workspace,false,"PartA.Mesh","PartB.Texture")
--Is capable of searching through multiple instances downwards.

--Example 3:
local PartA,PartB,PartC = Code.FindAllChildren(workspace,true,"PartA","PartB","PartC")
--Will return the instances if they exist anywhere in the game under workspace.
```

___
</details>

<details><summary>IsPartOf</summary>

**Description:** Checks if an Instance is a descendant of another Instance or of a certain type of Instance.

**Setup:** `Code.IsPartOf(Instance, Tuple, MaxReturn)`

**Returns:** The Instance that was found, or `nil` if it managed to hit **game** before finding anything or couldn't be found within the max amount of parents requested by the Number value.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | The instance you want to check if it's a part of something. |
| Tuple | Instance / string | nil | The ClassName or Instance you're looking for. |
| MaxReturn | number | :infinity: | The amount of parents it will check. |

### Usage Example

```lua
--Let's assume you set the Part variable already.
local PartInFolder = Code.IsPartOf(Part,"Folder")
--Will return either the Folder Instance we're looking for if it exists or nil if it doesn't.
--Let's assume you already have Part1 and Part2 set.
local Partception = Code.IsPartOf(Part1,Part2,2)
--Will return the 2nd part if Part1 is no more than 2 descendants down, otherwise nil.
```

___
</details>

<details><summary>GetPartOf</summary>

**Description:** Fetches a property from an array of instances.

**Setup:** `Code.GetPartOf(Instances, Property)`

**Returns:** A table of the given property.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instances | {Instance...} | {} | An array of Instances you want to check through. |
| Property | string | nil | The property you want returned. Can also be set to return a secondary value if one exists (like Vector3 with X,Y or Z). |

### Usage Example

```lua
--Example 1: Let's assume Parts is a folder under the workspace holding some bricks.
local Transparencies = Code.GetPartOf(Parts:GetChildren(),"Transparency")
print(unpack(Transparencies))

--Example 2: Let's fetch all of the Y positions of each part now.
local Y = Code.GetPartOf(Parts:GetChildren(),"Position.Y")
print(unpack(Y))
```

___
</details>

<details><summary>WaitForPath</summary>

**Aliases:** WaitForDescendants, WFP

**Description:** [A solution to checking in long paths without needing the overuse of :WaitForChild a dozen times.](https://devforum.roblox.com/t/554586)

**Setup:** `Code.WaitForPath(Instance, MaxWaitTime, Path)`

**Returns:** The Instance(s) you're looking for, or false if you exceed the maximum wait time and found nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | Where the search will start. |
| MaxWaitTime | number | 20 | The maximum wait time. |
| Path | string | nil | The path you're looking down. |

**Note:**
If before the name of the variable an ```*asterisk``` is placed anywhere in the sequence, that found instance will also be returned.

### Usage Example

```lua
--Let's assume that this is a LocalScript for some UI.
local UI = script.Parent
local Button = Code.WaitForPath(UI,20,"MainFrame.Something.SomethingElse.Button1")
--Will return "Button1" that would be down the path MainFrame.Something.SomethingElse

--If we also wanted to save MainFrame but don't want to repeat it:
local MainFrame,Button = Code.WaitForPath(UI,20,"*MainFrame.Something.SomethingElse.Button1")
--The asterisk before the name tells the function to also save that instance.
```

___
</details>

<details><summary>WaitForChildren</summary>

**Aliases:** WFC

**Description:** Allows you to call :WaitForChild() on multiple Instances under the same parent at the same time.

**Setup:** `Code.WaitForChildren(Instance, MaxWait, Items...)`

**Returns:** The Instance(s) you're looking for. Will return false for each Instance that fails to be found within the time you set.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | Where to look. |
| MaxWait | number | 20 | The maximum time allowed to wait on an Instance. |
| Items | string | nil | The items you would like to search for. |

**Note:**
If before the name of the variable an ```*asterisk``` is placed anywhere in the sequence, that found instance will also be returned.

### Usage Example

```lua
----Example 1
--Let's assume that this is a LocalScript for some UI.
local UI = script.Parent
local Frame1,Frame2,Button = Code.WaitForChildren(UI,10,"Frame1","Frame2","Button")
    --Will return the Instances that are within the UI, or false for each Instance
    --that cannot be found within 10 seconds.

----Example 2
local TextureA,TextureB = Code.WFC(workspace,10,"Brick1.Decal","Brick2.Texture")
    --Will return the decal/texture found, or false if not there.

----Example 3
local Brick1,TextureA,TextureB = Code.WFC(workspace,10,"*Brick1.Decal","Brick2.Texture")
    --Will return the first brick, then the decal/texture found, or false if not there.
```

___
</details>

<details><summary>Fetch</summary>

**Description:** Fetches multiple properties/values from an instance/table.

**Setup:** `Code.Fetch(Input, Variables...)`

**Returns:** The variable(s) you're looking for. If the variable cannot be found or is equal to `nil` then it will be overlooked.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Input | Instance / Dictionary | nil | What should be searched through. Anything that has a variable attached to it should work. |
| Variables | string | nil | To be fetched from the input. |

**Note:**
If a property is followed by ` > ` then it will search inside this property instead. Must have a space before and after this symbol.

If after a `>` there are any commas, these will be taken into consideration separately.

### Usage Example

```lua
----Example 1
local Color,Size = Code.Fetch(workspace.Baseplate,"Color","Size")
    --Can be followed by as many variables as necessary as long as the input actually has these things.

----Example 2
local Color,SizeX,SizeY,SizeZ = Code.Fetch(workspace.Baseplate,"Color","Size > X,Y,Z")
    --Will return each size axis after getting the part's color.
```

___
</details>

<details><summary>PositiveNegative</summary>

**Aliases:** PN

**Description:** Returns a 50/50 chance for a number being positive or negative.

**Setup:** `Code.PN(Number)`

**Returns:** The number generated, or ±1 if no number was set.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Number | number | 1 | The number that's being randomized. |

### Usage Example

```lua
--Example 1:
local Number = Code.PN(5)   --Returns either 5 or -5
--Example 2:
local Number = Code.PN()   --Returns either 1 or -1
```

___
</details>

<details><summary>Random</summary>

**Aliases:** Rando

**Description:** Selects at random whatever you put in the list.

**Setup:** `Code.Rando(Items...)`

**Returns:** One of the items you put in the list at random.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Items | Any | nil | The items you wish to input. Can be anything, really. |

### Usage Example

```lua
--Example 1:
local Number = Code.Rando(1,10,30,-6,1000)
--Example 2:
local Chosen = Code.Rando("Hello, world!",96,workspace.Brick)
```

___
</details>

<details><summary>Service</summary>

**Description:** Fetches one or more services.

**Setup:** `Code.Service(Service...)`

**Returns:** The service(s) that you requested.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Service | string | nil | The service(s) in which you'd like to fetch. |

### Usage Example

```lua
--Example 1:
local TS = Code.Service'TweenService'
--Example 2:
local TS,RS,SSS = Code.Service('TweenService','RunService','ServerScriptService')
```

___
</details>

<details><summary>Tween</summary>

**Description:** A simplified method of making a tween.

**Setup:** `Code.Tween(Instance){Time, Style, Direction, Repeat, Reverses, Delay}{Properties}`

**Returns:** The Tween you've created.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | The Instance in which you wish to tween. |
| Time | number | 1 | How long it takes the tween to complete. |
| Style | Enum / number / string | Linear | The EasingStyle of the tween. |
| Direction | Enum / number / string | InOut | The EasingDirection of the tween. |
| Repeat | number | 0 | How many times the tween will repeat. |
| Reverses | boolean | false | Determines if the tween will do the inverse after finishing. |
| Delay | number | 0 | The amount of time that elapses before tween starts in seconds. |
| Properties | table | {} | The properties that are being changed by the tween. Generally numerical. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
local Tween = Code.Tween(script.Parent){1,"Bounce","Out",1,true,0.5}{Position=script.Parent.Position+Vector3.new(0,3,0)}
Tween:Play()
```

___
</details>

<details><summary>Tweens</summary>

**Description:** A method for making multiple tweens of the same or similar items.

**Setup:** `Code.Tweens(Instances...){Time, Style, Direction, Repeat, Reverses, Delay}{Properties}`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instances | Instance | nil | The Instances in which you wish to tween. |
| Time | number | 1 | How long it takes the tween to complete. |
| Style | Enum / number / string | Linear | The EasingStyle of the tween. |
| Direction | Enum / number / string | InOut | The EasingDirection of the tween. |
| Repeat | number | 0 | How many times the tween will repeat. |
| Reverses | boolean | false | Determines if the tween will do the inverse after finishing. |
| Delay | number | 0 | The amount of time that elapses before tween starts in seconds. |
| Properties | table | {} | The properties that are being changed by the tween. Generally numerical. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
local Part1,Part2 = workspace.Part1,workspace.Part2
Code.Tweens(Part1,Part2){1,"Bounce","Out",1,true,0.5}{Position=Vector3.new(0,5,0)}
```

**Note:** This auto-plays all of the tweens made. I intended it to return each tween individually similarly to the Service function, but something ROBLOX-side seems to be preventing this from working as of this update.
___
</details>

<details><summary>⚠ TweenSequence</summary>

:warning: This is experimental, and may cause poor performance if used too sparingly!

**Description:** An experimental method to tween what was previously untweenable.

**Setup:** `Code.TweenSequence(Instances...){Time, Style, Direction, Repeat, Reverses, Delay}{Properties}`

**Returns:** A special tween-base made via metatables. Should be able to work just like a normal Tween with the same functions and variables. This includes a new function: **Tween:Destroy()** since this works in a specific way, if you want to clean up a bit, I recommend you use this when not needed anymore.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instances | Instance | nil | The Instances in which you wish to tween. |
| Time | number | 1 | How long it takes the tween to complete. |
| Style | Enum / number / string | Linear | The EasingStyle of the tween. |
| Direction | Enum / number / string | InOut | The EasingDirection of the tween. |
| Repeat | number | 0 | How many times the tween will repeat. |
| Reverses | boolean | false | Determines if the tween will do the inverse after finishing. |
| Delay | number | 0 | The amount of time that elapses before tween starts in seconds. |
| Properties | table | {} | The properties that are being changed by the tween. In this case, restricted. |

## **What can be tweened currently**

### ColorSequence

If the start and end sequences have differing number keypoints, 2 new sequences will be created with some "ghost keypoints" in the middle to still generally reflect what the start and end should look like. Then, when tweening, each color's keypoint is lerped over each other to give a fading effect.

If the number of keypoints remains the same and the start of the property name has a tilde (**~**), then it will attempt to also tween the time position of the keypoints between the start and end points, giving a sliding effect on top of the color changing effect.

### NumberSequence

Works largely similar to ColorSequence, but instead with colors it deals in numbers and envelopes. The tilde rule also applies here.

### Usage Example

```lua
----Example 1
local Beam = workspace.Part1.Beam
local ChangeTo = ColorSequence.new{
 ColorSequenceKeypoint.new(0,Color3.fromRGB(0, 0, 0)),
 ColorSequenceKeypoint.new(0.25,Color3.fromRGB(255, 0, 0)),
 ColorSequenceKeypoint.new(0.5,Color3.fromRGB(0, 255, 0)),
 ColorSequenceKeypoint.new(0.75,Color3.fromRGB(0, 0, 255)),
 ColorSequenceKeypoint.new(1,Color3.fromRGB(0, 0, 0))}

local Tween = Code.TweenSequence(Beam){1,"Linear","InOut",2,true,0.5}{Color = ChangeTo}
Tween:Play()
--Should tween any beam's colors to look a bit rainbow-like, reverts back, and does this a couple of times.

----Example 2
local ParticleEmitter = workspace.Part5.ParticleEmitter
local ChangeTo = NumberSequence.new{
 NumberSequenceKeypoint.new(0,1),
 NumberSequenceKeypoint.new(0.349,3.39,1.58),
 NumberSequenceKeypoint.new(1,1)}

local Tween = Code.TweenSequence(ParticleEmitter){1,"Linear","InOut",2,true,0.5}{["~Size"> = ChangeTo}
Tween:Play()
Tween.Completed:Wait()
print("Demo finished!")
--Should tween a particle emitter's size, sliding the values if the initial NumberSequence is also 3 keypoints.
```

___
</details>

<details><summary>Tabs</summary>

**Description:** Combines Tables or otherwise into 1 table.

**Setup:** `Code.Tabs(Any)`

**Returns:** The new table with everything inside.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Any | Any | {} | An array of anything you want to combine. Tables, Strings, Instances, etc. |

### Usage Example

```lua
--Example 1:
local Tab1,Tab2 = {1,3,5},{2,4,6}
local Tab3 = Code.Tabs(Tab1,Tab2)

print(unpack(Tab3)) --1 3 5 2 4 6
--Example 2:
local Tab = {1,2,3,4}
local Tab2 = Code.Tabs("Hi",false,workspace,Tab,"Test")
print(unpack(Tab2)) --Hi false Workspace 1 2 3 4 Test
```

___
</details>

<details><summary>TabClone</summary>

**Description:** Clones a table.

**Setup:** `Code.TabClone(Table)`

**Returns:** The new cloned table.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Table | table | nil | The table you want to clone. |

### Usage Example

```lua
local Tab1 = {1,2,3,"a","b","c"}
local Tab2 = Code.TabClone(Tab1)
print(Tab1==Tab2) --false
```

___
</details>

<details><summary>MassConnect</summary>

**Description:** A quick method for connecting multiple events and instances simultaneously.

**Setup:** `Code.MassConnect(Instances, Events, Function)`

**Returns:** An array of every new connection made.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instances | table | {} | An array of Instances you'd like to connect. |
| Events | table | {} | An array of events of the Instances you'd like to have connected. |
| Function | Function | nil | The function you'd like to be connected to the event(s). |

**Note:**
Function will always use 2 variables at the start: The Instance connected, and a string of the Event connected. After that, every variable that would be ordinarily returned via the event.
Format it as so:

```lua
function TestFunction(Instance,Event,...)
--Instance being the instance.
--Event is a string of the event you chose to connect.
local VarA,VarB,VarC,etc = ...
--or
local Variables = {...}
end
```

### Usage Example

```lua
--Let's assume this is a normal script, and Parts is a folder in the workspace that contains a few blocks.
function Reader(Part,Event,...)
    if Event=="Changed" then
    print(Part.Name,"was changed! Changed:",...)
    elseif Event=="Touched" then
    print(Part.Name,"was touched! Part that hit it:",...)
    end
end

Code.MassConnect(Parts:GetChildren(),{"Touched","Changed"},Reader)
```

___
</details>

<details><summary>MassDisconnect</summary>

**Description:** Disconnects a bunch of connections.

**Setup:** `Code.MassDisconnect(Connections)`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Connections | table | {} | An array of Connections that you'd like to sever. |

**Note:**
:warning: It's best to use this after using **MassConnect** when you don't need the connections any more.

### Usage Example

```lua
--Let's assume this is a normal script, and Parts is a folder in the workspace that contains a few blocks.
function Reader(Part,Event,...)
    if Event=="Changed" then
    print(Part.Name,"was changed! Changed:",...)
    elseif Event=="Touched" then
    print(Part.Name,"was touched! Part that hit it:",...)
    end
end

local Connections = Code.MassConnect(Parts:GetChildren(),{"Touched","Changed"},Reader)

task.wait(10)

Connections = Code.MassDisconnect(Connections)
--Every connection will be gone, and so is the reference to them.
```

___
</details>

<details><summary>Match</summary>

**Description:** A simple replacement for using multiple "or" statements on the same variable.

**Setup:** `Code.Match(Main, Variables...)`

**Returns:** A boolean value; true if the Main variable matches any of the subsequent variables or false if not.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Main | Any | nil | The variable you're reading off of. |
| Variables | Any | nil | The variables you'd like to compare it to. |

### Usage Example

```lua
--Let's assume Part is the variable set to a part under the workspace.

if Code.Match(Part.Transparency,0,0.5,1)  then
--This part of the code runs if the part's transparency is either 0, 0.5, or 1.
else
--Otherwise...
end
```

___
</details>

<details><summary>AllMatch</summary>

**Description:** A simple replacement for checking if all variables must equal the same thing.

**Setup:** `Code.AllMatch(MatchMe, Variables...)`

**Returns:** A boolean value; true if the following variables all match the MatchMe variable or false if not.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| MatchMe | Any | nil | The variable you want the rest to match. |
| Variables | Any | nil | The variables you'd like to compare it to. |

### Usage Example

```lua
local Result = Code.AllMatch(1,2,3,1)
--Result would be false since they all need to equal the first variable (1).
local Result = Code.AllMatch(1,1,1,1)
--Result would be true.
```

___
</details>

<details><summary>WaitOn</summary>

**Description:** Can wait on multiple occasions, but will resume as soon as 1 of them is met.

**Setup:** `Code.WaitOn(Variant...)`

**Returns:** The method that prevailed (only really applicable for those who understand in the case of an event being returned).
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Variant | Number / Signal / Function / Table | nil | The method of waiting you'd like to input |

**Note:**
Variant can be presented as any of the following:
♦ Number: The number of seconds you'd like to wait. Same as **wait(Number)**.
♦ Signal: An event such as **Instance.Changed**.
♦ Function: Your own method of waiting, I suppose. A function.
♦ Table {Name,Signal}: Name is a string that you'd want to be returned, and Signal is an event or function. Makes for easier identification.

### Usage Example

```lua
--Let's assume Part is the variable set to a part under workspace, and we want to wait till it gets changed at all, but we don't want to wait more than 10 seconds for that to happen.
Code.WaitOn(10,Part.Changed)
--However, if we want to wait on a specific property (Transparency in this case)...
Code.WaitOn(10,Part:GetPropertyChangedSignal("Transparency"))
--If we're using multiple of the same Events, we're gonna want to be able to tell the difference between them
Code.WaitOn({"Part1",Part1.Part:GetPropertyChangedSignal("Transparency")},{"Part2",Part2.Part:GetPropertyChangedSignal("Transparency")})
    --returns the string "Part1" if Part1's transparency changes, or "Part2" if Part2's transparency changes.
```

___
</details>

<details><summary>Require</summary>

**Description:** Requires multiple modules at once.

**Setup:** `Code.Require(Modules...)`

**Returns:** The modules you wanted to fetch.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Modules | ModuleScript | nil | The module(s) you'd like to require. |

### Usage Example

```lua
local A,B,C = Code.Require(ModuleA,ModuleB,ModuleC)
```

___
</details>

<details><summary>Make</summary>

**Description:** Quickly make multiple of the same thing.

**Setup:** `Code.Make(Data, Amount, SameTable)`

**Returns:** The data you've copied.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Data | Any | nil | The number, string, vector3, etc that you're copying. |
| Amount | number | 0 | How many times it will copy. |
| SameTable | boolean | false | Only applicable if the Data is a table. Determines if the returned tables are all the same connected one or just copies. |

### Usage Example

```lua
----Example 1 (any data):
local A,B,C = Code.Make("Test",3)
print(A,B,C) --Test, Test, Test
----Example 2 (connected tables):
local Tab = {"A","b"}
local T1,T2 = Code.Make(Tab,2,true)
print(T1==T2) --true
----Example 2 (unconnected tables):
local Tab = {"A","b"}
local T1,T2 = Code.Make(Tab,2)
print(T1==T2) --false
```

___
</details>

<details><summary>TimeTable</summary>

**Description:** Takes any number of seconds and returns a dictionary of all possible conversions from the formula **TimeConvert**.

**Setup:** `Code.TimeTable(Seconds)`

**Returns:** A dictionary of time conversions.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Seconds | number | 0 | How many seconds you want to convert. |

### Usage Example

```lua
--Converts 65 seconds into a table that returns 1 minute and 5 seconds.
local Times = Code.TimeTable(65)
print(Times)
--[[Output: {
    ["Days"] = 0,
    ["Hours"] = 0,
    ["Milliseconds"] = 0,
    ["Minutes"] = 1,
    ["Seconds"] = 5,
    ["Weeks"] = 0
    }]]
```

___
</details>

<details><summary>TimeFormat</summary>

**Description:** Takes any number of seconds and converts that to a time format of your choosing.

**Setup:** `Code.TimeFormat(Seconds, Format, Simple)`

**Returns:** A string containing the newly formatted time.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Seconds | number | 0 | How many seconds you want to convert and format. |
| Format | string | **nil** | How the string should be presented. See below category for more details. |
| Simple | boolean | true | If the conversion can be used without any words in the string. |

### Format Guides
The string for the **Format** variable looks for certain patterns to determine where to place the converted time units. This goes to Milliseconds, Seconds, Minutes, Hours (24 hour, not 12 hour time), Days, and Weeks. Months and onward are excluded for being too variable; not every month has a set amount of days or weeks. When using these patterns, keep in mind that these are case sensitive:

| Pattern | Time Unit | Number Range |
| --- | --- | --- |
| Mi | Milliseconds | 0 - 9999 |
| S | Seconds | 0 - 59 |
| M | Minutes | 0 - 59 |
| H | Hours | 0 - 23 |
| D | Days | 0 - 6 |
| W | Weeks | No Limit |

Units such as Seconds or Hours do not display their limits of 60 or 24 since at that number they will convert to the next unit of time. 60 seconds to +1 minute.

Note that when the **Simple** variable is set to *false*, you will instead need to use every pattern with an underscore before it. So **Mi** will now be **_Mi**. See Usage Examples to see why this can be useful.

### Usage Example

```lua
--Example 1: Simple timer conversion for 65 seconds.
local Time = Code.TimeFormat(65, "MM:SS.MiMiMi", true)
print(Time)
    --Expected output: "01:05.000"

--Example 2: Take's Example 1 but removes the excess zero from the minutes, and removes the milliseconds.
local Time = Code.TimeFormat(65, "M:SS")
print(Time)
    --Expected output: "1:05"
    --Note that the third variable is true by default, so it is unecessary to include here.

--Example 3: Displays 3 Hours  1 Minute  20.5 Seconds
local Time = Code.TimeFormat(60^2 * 3 + 80.5, "HH:MM:SS.MiMiMi")
print(Time)
    --Expected output: "03:01:20.500"
 
 --Example 4: How to utilize the third variable to be false to display more accurate information. This will display 6 days  1 Minute  20 Seconds
local Time = Code.TimeFormat(60^2 * 24 * 6 + 80, "_D Days _HH:_MM:_SS._MiMiMi", false)
print(Time)
    --Expected output: "6 Days 00:01:20.000"
    --Had we not set the Simple variable to false, this function would have attempted to convert the word Days to a number for the number of Days as well since it starts with a D.
```

___
</details>

<details><summary>Plugin_Settings</summary>

**Description:** Fetches plugin settings. For Plugins only.

**Setup:** `Code.Plugin_Settings(Plugin, Setting...)`

**Returns:** The setting(s) you wanted to fetch or their defaults if they didn't exist.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Plugin | plugin | nil | Simply pass the plugin variable through. |
| Setting | {Type, Default} | nil | The setting(s) that you're trying to fetch. |

**Note:**
The Setting table must be set up as:
♦ **Type:** The name you want to set/fetch the setting from in string form.
♦ **Default:** If the setting could not be found, this is what it will be defaulted to instead.

### Usage Example

```lua
local SettingA,SettingB = Code.Plugin_Settings(plugin,{"Color",Color3.new(1,1,1)},{"Word","Hello!"})
-- If either SettingA or B did not exist, the setting for their 1st value in the table, it would be set to the 2nd value in the pair.
```

___
</details>

<details><summary>Plugin_Widget</summary>

**Description:** A simplified method of making a plugin widget. For Plugins only.

**Setup:** `Code.Plugin_Widget(Plugin, Identifier, DisplayName){InitialDockState, InitialEnabled, RestoreOverride, SizeX, SizeY, SizeMinimumX, SizeMinimumY}`

**Returns:** The widget you've created.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Plugin | plugin | nil | Simply pass the plugin variable through. |
| Identifier | string | nil | The plugin widget's ID. |
| DisplayName | string | nil | The name shown on the widget window. |
| InitialDockState | Enum / string / number | Float | Initial dock state. |
| InitialEnabled | boolean | true | If the widget is visible upon being made. |
| RestoreOverride | boolean | false | If true, the value of **InitialEnabled** will override the previously saved enabled state. |
| SizeX | number | 200 | Initial width of the widget window. |
| SizeY | number | 300 | Initial height of the widget window. |
| SizeMinimumX | number | 150 | Minimum width of the widget window. |
| SizeMinimumY | number | 150 | Minimum heightof the widget window. |

### Usage Example

```lua
local Widget = Code.Plugin_Widget(plugin,"TestWidget","Test Widget"){"Float",true,false,200,300,150,150}
-- Creating a basic test widget with basically the default values in place.
```

___
</details>

<details><summary>SetAttributes</summary>

**Aliases:** SetAtt, SetAtts

**Description:** Sets multiple attributes at once.

**Setup:** `Code.SetAttributes(Instance, Table)`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | The Instance that you'd like to add an attribute on to. |
| Table | table | nil | A table listing the new attributes and their values. |

### Usage Example

```lua
local Part = workspace.Brick
Code.SetAttributes(Part,{TestString="String!",TestNumber=5,TestBoolean=false})
```

___
</details>

<details><summary>GetAttributes</summary>

**Aliases:** GetAtt, GetAtts

**Description:** Gets multiple specific attributes at once (in order).

**Setup:** `Code.GetAttributes(Instance, AutoMake, Attribute...)`

**Returns:** The attribute's value or **false** if none exists.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | The Instance that you'd like to find the attribute of. |
| AutoMake | boolean | true | If Attribute is a table input and if the attribute is missing, automatically create one and return its value. |
| Attribute | string / table | nil | The attribute(s) you want to fetch. |

**Notes:**
AutoMake can be skipped over.

Attribute's table is meant to be set up as follows: `{Name, Preset}`
♦ Name: The name of the attribute.
♦ Preset: The default value of the attribute if it doesn't yet exist.
If you're using the table variant, if there is a found attribute that does not match the type/typeof the preset, then the preset will be used instead.

### Usage Example

```lua
----Example 1
local Part = workspace.Brick
Code.SetAttributes(Part,{TestString="String!",TestNumber=5,TestBoolean=true})
local A,B,C,D = Code.GetAttributes(Part,"TestNumber","TestString","TestBrick","TestBoolean")
--A would be 5
--B would be "String!"
--C would be false since there is no match
--D would be true

----Example 2
local Part = workspace.Brick
local A,B = Code.GetAttributes(Part,{"TestNumber",5},"TestString")
--A would be 5 if there wasn't one already set
--B would be false assuming one wasn't there

----Example 3
local Part = workspace.Brick
local A,B = Code.GetAttributes(Part,false,{"TestNumber",5},"TestString")
--A would be false if it doesn't already exist since the AutoMake variable was set to false
--B would be false assuming one wasn't there
```

___
</details>

<details><summary>ClearAttributes</summary>

**Aliases:** ClearAtt, ClearAtts, NoAtt

**Description:** Clears multiple attributes at once.

**Setup:** `Code.ClearAttributes(Instance, Attribute...)`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | nil | The Instance that you'd like to clean up. |
| Attribute | string / nil | nil |  |

### Usage Example

```lua
----Taking from the "SetAttributes" example above:
--Clearing specific ones:
local Part = workspace.Brick
Code.SetAttributes(Part,{TestString="String!",TestNumber=5,TestBoolean=false})
wait(5)
Code.ClearAttributes(Part,"TestString","TestBoolean") --in this case, TestNumber remains
--Clearing all:
local Part = workspace.Brick
Code.SetAttributes(Part,{TestString="String!",TestNumber=5,TestBoolean=false})
wait(5)
Code.ClearAttributes(Part) --in this case, none remains
```
</details>

#

</details>

<details><summary><h3>Special Inputs List</h3></summary>

This list is specifically for the functions where changing the properties of an Instance is possible.

### On Changing Values

| Name | Type | Effects | Description | Example |
| --- | --- | --- | --- | --- |
| Same# | Property Name | Any | Allows you to set multiple properties to the same value. Provide a table where the first entry is the value you want to be set, then each subsequent entry is a string that is the name of the property you want to change. As long as the **#** at the end is a different number, you can use this as many times as necessary. | {Same1 = {true, "Anchored", "CanCollide"}, Same2 = {false,"CastShadow", "Locked"}} |
| Sides | Property Name | Surfaces | Changes all of the sides for BaseParts at the same time. | {Sides = "Smooth"} |
| "nil" | Value | ObjectValues | Allows you to set a property to `nil` where applicable. | {Adornee = "nil"} |
| "+#" | Value | Numbers | Adds the number **#** to the original value being changed. | {Value = "+0.5"} |
| "-#" | Value | Numbers | Subtracts the number **#** to the original value being changed. | {Value = "-0.5"} |
| "*#" | Value | Numbers | Multiplies the number **#** with the original value being changed. | {Value = "*2"} |
| "/#" | Value | Numbers | Divides the number **#** from the original value being changed. | {Value = "/2"} |
| "^#" | Value | Numbers | Sets the original value to the power of number **#**. | {Value = "^2"} |
| "sqrt" | Value | Numbers | Returns the square root of the number being changed. | {Value = "sqrt"} |
| "negate" | Value | Numbers | Basically inverts the number from negative/positive to the other. | {Value = "negate"} |
| "not" | Value | Booleans | Returns the opposite boolean value. | {Value = "not"} |
| Function | Value | Any | Inputs a function to return a value of your choosing. The first/only value of the function is always the Instance being changed. | See [Expanded Examples](#expanded-examples) for this instance. |
| "~X,Y,Z" | Value | Vector3 | Changes the Vector3 relative to its current value. The **X**, **Y**, and **Z** variables are the X,Y, and Z of the Vector3 Value. No spaces should be present in here. | {Position = "~0,5,0"} |
| **"~#,#,#"** | Value | CFrame | Translates as ToWorldSpace(CFrame). | {CFrame = "~0,0,-5"} |
| **"@#,#,#"** | Value | CFrame | Translates as CFrame*CFrame.fromEulerAnglesXYZ(#,#,#). Automatically converted to math.rad(#). | {CFrame = "@90,0,45"} |
| **"<#,#,#,#,#,#"** | Value | CFrame | Basically acts as the **~** and then applies the **@** changes. First three #'s affect the movement and the other three affect the rotation. | {CFrame = "<0,5,0,0,45,0"} |
| **">#,#,#,#,#,#"** | Value | CFrame | Basically acts as the **@** and then applies the **~** changes. First three #'s affect the movement and the other three affect the rotation. | {CFrame = ">0,5,0,0,45,0"} |

### Expanded Examples

```lua
--Function
--Moves each part in your PartTable 5 studs up independently of each other and their rotation.
function Func(Part)
return Part.CFrame:ToWorldSpace(CFrame.new(0,5,0))
end

Code.Change(PartTable){CFrame = Func}
```

### On Searching Values

| Name | Type | Effects | Description | Example |
| --- | --- | --- | --- | --- |
| IsA | Property Name | Instances | Works just like **Part:IsA("BasePart"). | {IsA = "BasePart"} |
| IsPartOf | Property Name | Instances | Works with this module's **IsPartOf** function, though requires a slightly different setup. | See [Expanded Examples](#expanded-examples-1) for this instance. |
| Attribute | Property Name | Instances | Looks for 1 attribute to match to. | {Attribute = {"Attribute Name", DesiredValue} |
| ">#" | Value | Numbers | Detects anything greater than the **#** in its place. | {Transparency = ">0.5"} |
| ">=#" | Value | Numbers | Detects anything greater or equal to the **#** in its place. | {Transparency = ">=0.5"} |
| "<#" | Value | Numbers | Detects anything lower than the **#** in its place. | {Transparency = "<0.5"} |
| "<=#" | Value | Numbers | Detects anything lower or equal to the **#** in its place. | {Transparency = "<=0.5"} |
| ...String | Value | Strings | Checks if the desired String is at the end of the value. | {Name = "...Brick"} |
| String... | Value | Strings | Checks if the desired String is at the start of the value.  | {Name = "Brick..."}  |
| ...String... | Value | Strings | Checks if the desired String is anywhere inside of the value.  | {Name = "...Brick..."}  |
| "R" | Value | Color3 | Checks if the **R** value is greater than **B** and **G** . | {Color = "R"} |
| "G" | Value | Color3 | Checks if the **G** value is greater than **R** and **B** . | {Color = "G"} |
| "B" | Value | Color3 | Checks if the **B** value is greater than **R** and **G** . | {Color = "B"} |
| "RG" | Value | Color3 | Checks if the color is more **yellow** than **blue** . | {Color = "RG"} |
| "GB" | Value | Color3 | Checks if the color is more **cyan** than **red** . | {Color = "GB"} |
| "RB" | Value | Color3 | Checks if the color is more **purple** than **green** . | {Color = "RB"} |
| "...Name" | Value | BrickColor | Checks if the name of the **BrickColor** ends with your input. | {BrickColor = "...red"} |
| {Value,Value... etc} | Value | Any | You can now set each property to equal a table of values. Basically: If property equals this, or this, or this… etc. | {Transparency = {0, 0.5, 1}} |

### Expanded Examples

```lua
--IsPartOf 
--Returns the first BasePart it finds that is also the direct child of a Folder Instance.
--Check the IsPartOf function of this module to see the general set-up.
local Get = Code.Find(workspace,true,true){IsA="BasePart",IsPartOf={{"Folder",1}}}

```

</details>

___
If there is any misinformation with the functions on here, or you believe that something needs more clarification, please let me know.

Also, feel free to comment if you have **ideas** for any new functions that I should add that you believe would be useful.
