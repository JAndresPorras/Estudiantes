
USE [master]
GO
/****** Object:  Database [Estudiantes_Santel]    Script Date: 14/10/2022 11:40:41 ******/
CREATE DATABASE [Estudiantes_Santel]
 CONTAINMENT = NONE
 ON  PRIMARY 
( NAME = N'Estudiantes_Santel', FILENAME = N'/data/Estudiantes_Santel.mdf' , SIZE = 3264KB , MAXSIZE = UNLIMITED, FILEGROWTH = 65536KB )
 LOG ON 
( NAME = N'Estudiantes_Santel_log', FILENAME = N'/data/log/Estudiantes_Santel_log.ldf' , SIZE = 16896KB , MAXSIZE = 2048GB , FILEGROWTH = 65536KB )
 WITH CATALOG_COLLATION = DATABASE_DEFAULT
GO
ALTER DATABASE [Estudiantes_Santel] SET COMPATIBILITY_LEVEL = 150
GO
IF (1 = FULLTEXTSERVICEPROPERTY('IsFullTextInstalled'))
begin
EXEC [Estudiantes_Santel].[dbo].[sp_fulltext_database] @action = 'enable'
end
GO
ALTER DATABASE [Estudiantes_Santel] SET ANSI_NULL_DEFAULT OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET ANSI_NULLS OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET ANSI_PADDING OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET ANSI_WARNINGS OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET ARITHABORT OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET AUTO_CLOSE ON 
GO
ALTER DATABASE [Estudiantes_Santel] SET AUTO_SHRINK ON 
GO
ALTER DATABASE [Estudiantes_Santel] SET AUTO_UPDATE_STATISTICS ON 
GO
ALTER DATABASE [Estudiantes_Santel] SET CURSOR_CLOSE_ON_COMMIT OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET CURSOR_DEFAULT  GLOBAL 
GO
ALTER DATABASE [Estudiantes_Santel] SET CONCAT_NULL_YIELDS_NULL OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET NUMERIC_ROUNDABORT OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET QUOTED_IDENTIFIER OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET RECURSIVE_TRIGGERS OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET  ENABLE_BROKER 
GO
ALTER DATABASE [Estudiantes_Santel] SET AUTO_UPDATE_STATISTICS_ASYNC OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET DATE_CORRELATION_OPTIMIZATION OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET TRUSTWORTHY OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET ALLOW_SNAPSHOT_ISOLATION OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET PARAMETERIZATION SIMPLE 
GO
ALTER DATABASE [Estudiantes_Santel] SET READ_COMMITTED_SNAPSHOT OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET HONOR_BROKER_PRIORITY OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET RECOVERY SIMPLE 
GO
ALTER DATABASE [Estudiantes_Santel] SET  MULTI_USER 
GO
ALTER DATABASE [Estudiantes_Santel] SET PAGE_VERIFY CHECKSUM  
GO
ALTER DATABASE [Estudiantes_Santel] SET DB_CHAINING OFF 
GO
ALTER DATABASE [Estudiantes_Santel] SET FILESTREAM( NON_TRANSACTED_ACCESS = OFF ) 
GO
ALTER DATABASE [Estudiantes_Santel] SET TARGET_RECOVERY_TIME = 60 SECONDS 
GO
ALTER DATABASE [Estudiantes_Santel] SET DELAYED_DURABILITY = DISABLED 
GO
ALTER DATABASE [Estudiantes_Santel] SET ACCELERATED_DATABASE_RECOVERY = OFF  
GO
ALTER DATABASE [Estudiantes_Santel] SET QUERY_STORE = OFF
GO
USE [Estudiantes_Santel]
GO
/****** Object:  User [sa_aretama]    Script Date: 14/10/2022 11:40:42 ******/
CREATE USER [sa_aretama] FOR LOGIN [sa_aretama] WITH DEFAULT_SCHEMA=[dbo]
GO
ALTER ROLE [db_datareader] ADD MEMBER [sa_aretama]
GO
ALTER ROLE [db_datawriter] ADD MEMBER [sa_aretama]
GO
/****** Object:  Schema [Master]    Script Date: 14/10/2022 11:40:42 ******/
CREATE SCHEMA [Master]
GO
/****** Object:  Schema [Students]    Script Date: 14/10/2022 11:40:42 ******/
CREATE SCHEMA [Students]
GO
/****** Object:  UserDefinedFunction [Master].[Fn_GetMenuStandard]    Script Date: 14/10/2022 11:40:42 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
/*	
	SELECT [Master].[Fn_GetMenuStandard](1,'ReporteMain')
*/
CREATE FUNCTION [Master].[Fn_GetMenuStandard] (@ConvertINT INT, @TypeGroup VARCHAR(100) = 'STATUSDEF')

RETURNS VARCHAR(MAX)

AS
BEGIN

	DECLARE @ResultHTML NVARCHAR(MAX) = '';
			

	
	    IF(@TypeGroup = 'Estudiantes')	
		BEGIN
			SET @ResultHTML =  REPLACE(REPLACE(REPLACE('<div class="btn-group">
			  <button type="button" class="btn btn-primary dropdown-toggle removecaret" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false"> __RowID__ &nbsp;
				<i class="fas fa-list"></i>
			  </button>
			  <div class="dropdown-menu">
				<a class="dropdown-item" href="__Url__"><i class="fa fa-edit"></i> Editar</a>	
				
				<a class="dropdown-item" href="__Url2__"><i class="fa fa-trash"></i> Eliminar</a>
			  </div>
			</div>','__Url__','/Home/Notas?RowID=__RowID__'),'__Url2__', 'javascript:DeleteEstudiante(__RowID__)' ),'__RowID__',@ConvertINT)			
		END	
		ELSE IF(@TypeGroup = 'Notas')
			BEGIN
				SET @ResultHTML =  REPLACE(REPLACE('<div class="btn-group">
				  <button type="button" class="btn btn-primary dropdown-toggle removecaret" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false"> __RowID__ &nbsp;
					<i class="fas fa-list"></i>
				  </button>
				  <div class="dropdown-menu">
					<a class="dropdown-item" onclick="__Url__"><i class="fa fa-edit"></i> Editar</a>	
				  </div>
				</div>','__Url__','NotasEditar(__RowID__)'),'__RowID__',@ConvertINT)		
			END

	RETURN 
	(
		SELECT @ResultHTML
	)
		  
END
GO
/****** Object:  Table [Master].[Option]    Script Date: 14/10/2022 11:40:42 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [Master].[Option](
	[RowID] [int] IDENTITY(1,1) NOT NULL,
	[GroupID] [int] NULL,
	[Code] [varchar](70) NULL,
	[Name] [varchar](200) NULL,
	[Description] [varchar](1000) NULL,
	[Status] [bit] NOT NULL,
	[CreationUser] [varchar](50) NOT NULL,
	[CreationDate] [datetime] NOT NULL,
	[ModificationUser] [varchar](50) NULL,
	[ModificationDate] [datetime] NULL,
	[Auxiliary1] [int] NULL,
	[Auxiliary2] [bit] NULL,
	[Auxiliary3] [varchar](500) NULL,
	[Auxiliary4] [float] NULL,
	[Auxiliary5] [text] NULL,
	[Auxiliary6] [bigint] NULL,
	[Auxiliary7] [decimal](18, 2) NULL,
	[Auxiliary8] [smallint] NULL,
	[Auxiliary9] [money] NULL,
	[Auxiliary10] [text] NULL,
 CONSTRAINT [PK_Opcion] PRIMARY KEY CLUSTERED 
(
	[RowID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
/****** Object:  Table [Students].[Estudiantes]    Script Date: 14/10/2022 11:40:42 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [Students].[Estudiantes](
	[RowID] [int] IDENTITY(1,1) NOT NULL,
	[Nombres] [varchar](50) NOT NULL,
	[Apellidos] [varchar](50) NOT NULL,
	[Direccion] [varchar](100) NULL,
	[Grado] [int] NOT NULL,
	[Edad] [int] NOT NULL,
	[Sexo] [int] NOT NULL,
	[Comentario] [varchar](400) NULL,
	[UsuarioCreador] [varchar](50) NOT NULL,
	[FechaCreacion] [datetime] NOT NULL,
	[UsuarioModificador] [varchar](50) NULL,
	[FechaModificacion] [datetime] NULL,
 CONSTRAINT [PK_Estudiantes] PRIMARY KEY CLUSTERED 
(
	[RowID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
/****** Object:  View [Students].[vw_Estudiantes_List]    Script Date: 14/10/2022 11:40:42 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

--Creado por: Juan Andrés Porras
--Fecha creación: 14/10/2022
--Descripción: Vista para la tabla [Students].[Estudiantes]
--Modificado por:
--Fecha modificación:
--____________________________________

/*
	SELECT * FROM [Students].[vw_Estudiantes_List]
*/
CREATE VIEW [Students].[vw_Estudiantes_List]

AS
SELECT ISNULL([Estudent].[RowID],0)							AS [RowID]
      ,ISNULL([Estudent].[Nombres],'')						AS [Nombres]
      ,ISNULL([Estudent].[Apellidos],'')					AS [Apellidos]
      ,ISNULL([Estudent].[Direccion],'')					AS [Direccion]
      ,ISNULL([Estudent].[Grado],0)							AS [GradoID]
	  ,ISNULL([Grado].[Name],'')							AS [GradoName]
      ,ISNULL([Estudent].[Edad],0)							AS [Edad]
      ,ISNULL([Estudent].[Sexo],0)							AS [SexoID]
	  ,ISNULL([Sexo].[Name],0)								AS [SexoName]
      ,ISNULL([Estudent].[Comentario],'')					AS [Comentario]
      ,ISNULL([Estudent].[UsuarioCreador],'Admin')			AS [UsuarioCreador]
      ,ISNULL([Estudent].[FechaCreacion],NULL)				AS [FechaCreacion]
      ,ISNULL([Estudent].[UsuarioModificador],'')			AS [UsuarioModificador]
      ,ISNULL([Estudent].[FechaModificacion],NULL)			AS [FechaModificacion]
  FROM [Students].[Estudiantes] AS [Estudent]
	
	LEFT JOIN [Master].[Option] AS [Grado]
		ON [Estudent].[Grado] = [Grado].[RowID]

	LEFT JOIN [Master].[Option] AS [Sexo]
		ON [Estudent].[Sexo] = [Sexo].[RowID]
GO
/****** Object:  Table [Students].[Notas]    Script Date: 14/10/2022 11:40:42 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [Students].[Notas](
	[RowID] [int] IDENTITY(1,1) NOT NULL,
	[EstudianteID] [int] NOT NULL,
	[Materia] [int] NOT NULL,
	[Jornada] [int] NOT NULL,
	[HorasSemana] [int] NOT NULL,
	[Nota] [int] NULL,
	[Comentario] [varchar](400) NULL,
	[UsuarioCreador] [varchar](50) NOT NULL,
	[FechaCreacion] [datetime] NOT NULL,
	[UsuarioModificador] [varchar](50) NULL,
	[FechaModificacion] [datetime] NULL,
 CONSTRAINT [PK_Notas] PRIMARY KEY CLUSTERED 
(
	[RowID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
/****** Object:  View [Students].[vw_Notas_List]    Script Date: 14/10/2022 11:40:42 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
--Creado por: Juan Andrés Porras
--Fecha creación: 14/10/2022
--Descripción: Vista para la tabla [Students].[Notas]
--Modificado por:
--Fecha modificación:
--____________________________________

/*
	SELECT * FROM [Students].[vw_Notas_List]
*/

CREATE VIEW [Students].[vw_Notas_List]

AS
SELECT ISNULL([Notas].[RowID],0)						AS [RowID]
      ,ISNULL([Notas].[EstudianteID],0)					AS [EstudianteID]
      ,ISNULL([Notas].[Materia],0)						AS [MateriaID]
	  ,ISNULL([Materia].[Name],0)						AS [MateriaName]
      ,ISNULL([Notas].[Jornada],0)						AS [JornadaID]
	  ,ISNULL([Jornada].[Name],0)						AS [JornadaName]
      ,ISNULL([Notas].[HorasSemana],0)					AS [HorasSemana]
      ,ISNULL([Notas].[Nota],0)							AS [Nota]
      ,ISNULL([Notas].[Comentario],'')					AS [Comentario]
      ,ISNULL([Notas].[UsuarioCreador],'Admin')			AS [UsuarioCreador]
      ,ISNULL([Notas].[FechaCreacion],NULL)				AS [FechaCreacion]
      ,ISNULL([Notas].[UsuarioModificador],'')			AS [UsuarioModificador]
      ,ISNULL([Notas].[FechaModificacion],NULL)			AS [FechaModificacion]
  FROM [Students].[Notas] AS [Notas]

	LEFT JOIN [Master].[Option] AS [Materia]
		ON [Notas].[Materia] = [Materia].[RowID]

	LEFT JOIN [Master].[Option] AS [Jornada]
		ON [Notas].[Jornada] = [Jornada].[RowID]
GO
/****** Object:  Table [Master].[Group]    Script Date: 14/10/2022 11:40:42 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [Master].[Group](
	[RowID] [int] IDENTITY(1,1) NOT NULL,
	[Name] [varchar](80) NOT NULL,
	[Code] [varchar](35) NULL,
	[Description] [varchar](max) NULL,
	[Status] [bit] NOT NULL,
	[CreationUser] [varchar](50) NOT NULL,
	[CreationDate] [datetime] NOT NULL,
	[ModificationUser] [varchar](50) NULL,
	[ModificationDate] [datetime] NULL,
 CONSTRAINT [PK_Agrupacion] PRIMARY KEY CLUSTERED 
(
	[RowID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
SET IDENTITY_INSERT [Master].[Group] ON 
GO
INSERT [Master].[Group] ([RowID], [Name], [Code], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate]) VALUES (1, N'Grados', N'Grados', N'Grados del colegio Santel', 1, N'AndresPorras', CAST(N'2022-10-14T08:50:56.473' AS DateTime), NULL, NULL)
GO
INSERT [Master].[Group] ([RowID], [Name], [Code], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate]) VALUES (2, N'Sexo', N'Sexo', N'Genero masculino y femenino', 1, N'AndresPorras', CAST(N'2022-10-14T08:54:17.660' AS DateTime), NULL, NULL)
GO
INSERT [Master].[Group] ([RowID], [Name], [Code], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate]) VALUES (3, N'Jornada', N'Jornada', N'Jornada', 1, N'AndresPorras', CAST(N'2022-10-14T08:59:32.953' AS DateTime), NULL, NULL)
GO
INSERT [Master].[Group] ([RowID], [Name], [Code], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate]) VALUES (4, N'Materias', N'Materias', N'Materias', 1, N'AndresPorras', CAST(N'2022-10-14T09:01:29.863' AS DateTime), NULL, NULL)
GO
SET IDENTITY_INSERT [Master].[Group] OFF
GO
SET IDENTITY_INSERT [Master].[Option] ON 
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (1, 1, N'Grado', N'1', N'Primer grado', 1, N'AndresPorras', CAST(N'2022-10-14T08:53:04.093' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (2, 1, N'Grado', N'2', N'Segundo grado', 1, N'AndresPorras', CAST(N'2022-10-14T08:53:18.837' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (3, 1, N'Grado', N'3', N'Tercer grado', 1, N'AndresPorras', CAST(N'2022-10-14T08:53:30.570' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (4, 1, N'Grado', N'4', N'Cuarto grado', 1, N'AndresPorras', CAST(N'2022-10-14T08:53:41.320' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (5, 1, N'Grado', N'5', N'Quinto grado', 1, N'AndresPorras', CAST(N'2022-10-14T08:53:51.120' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (6, 2, N'Masculino', N'Masculino', N'Masculino', 1, N'AndresPorras', CAST(N'2022-10-14T08:54:38.100' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (7, 2, N'Femenino', N'Femenino', N'Femenino', 1, N'AndresPorras', CAST(N'2022-10-14T08:54:47.913' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (8, 4, N'Español', N'Español', N'Español', 1, N'AndresPorras', CAST(N'2022-10-14T08:56:08.320' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (9, 4, N'Sociales', N'Sociales', N'Sociales', 1, N'AndresPorras', CAST(N'2022-10-14T08:56:29.183' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (10, 4, N'Matemáticas', N'Matemáticas', N'Matemáticas', 1, N'AndresPorras', CAST(N'2022-10-14T08:56:58.513' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (11, 4, N'Cálculo', N'Cálculo', N'Cálculo', 1, N'AndresPorras', CAST(N'2022-10-14T08:57:19.347' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (12, 1, N'Grado', N'10', N'Décimo', 1, N'AndresPorras', CAST(N'2022-10-14T08:58:13.777' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (13, 1, N'Grado', N'11', N'Undécimo', 1, N'AndresPorras', CAST(N'2022-10-14T08:58:41.270' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (14, 3, N'Diurna', N'Diurna', N'Diurna', 1, N'AndresPorras', CAST(N'2022-10-14T09:02:11.727' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
INSERT [Master].[Option] ([RowID], [GroupID], [Code], [Name], [Description], [Status], [CreationUser], [CreationDate], [ModificationUser], [ModificationDate], [Auxiliary1], [Auxiliary2], [Auxiliary3], [Auxiliary4], [Auxiliary5], [Auxiliary6], [Auxiliary7], [Auxiliary8], [Auxiliary9], [Auxiliary10]) VALUES (15, 3, N'Nocturna', N'Nocturna', N'Nocturna', 1, N'AndresPorras', CAST(N'2022-10-14T09:02:23.337' AS DateTime), NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL)
GO
SET IDENTITY_INSERT [Master].[Option] OFF
GO
SET IDENTITY_INSERT [Students].[Estudiantes] ON 
GO
INSERT [Students].[Estudiantes] ([RowID], [Nombres], [Apellidos], [Direccion], [Grado], [Edad], [Sexo], [Comentario], [UsuarioCreador], [FechaCreacion], [UsuarioModificador], [FechaModificacion]) VALUES (7, N'Juan Andrés', N'Porras', N'CR 102 # 70-76', 12, 22, 6, N'Prueba final', N'Prueba', CAST(N'2022-10-14T16:15:05.277' AS DateTime), NULL, NULL)
GO
INSERT [Students].[Estudiantes] ([RowID], [Nombres], [Apellidos], [Direccion], [Grado], [Edad], [Sexo], [Comentario], [UsuarioCreador], [FechaCreacion], [UsuarioModificador], [FechaModificacion]) VALUES (9, N'Fred', N'Molina C', N'cr 102 70-76', 12, 25, 6, N'Pruba Total de Data Json', N'Prueba', CAST(N'2022-10-14T16:17:47.573' AS DateTime), NULL, NULL)
GO
SET IDENTITY_INSERT [Students].[Estudiantes] OFF
GO
SET IDENTITY_INSERT [Students].[Notas] ON 
GO
INSERT [Students].[Notas] ([RowID], [EstudianteID], [Materia], [Jornada], [HorasSemana], [Nota], [Comentario], [UsuarioCreador], [FechaCreacion], [UsuarioModificador], [FechaModificacion]) VALUES (1, 4, 8, 15, 4, 8, N'Prueba Nota 3', N'Prueba', CAST(N'2022-10-14T12:13:25.313' AS DateTime), N'Prueba', CAST(N'2022-10-14T15:59:47.047' AS DateTime))
GO
INSERT [Students].[Notas] ([RowID], [EstudianteID], [Materia], [Jornada], [HorasSemana], [Nota], [Comentario], [UsuarioCreador], [FechaCreacion], [UsuarioModificador], [FechaModificacion]) VALUES (2, 4, 9, 15, 4, 4, N'Prueba de edición', N'Prueba', CAST(N'2022-10-14T16:01:56.043' AS DateTime), NULL, NULL)
GO
INSERT [Students].[Notas] ([RowID], [EstudianteID], [Materia], [Jornada], [HorasSemana], [Nota], [Comentario], [UsuarioCreador], [FechaCreacion], [UsuarioModificador], [FechaModificacion]) VALUES (3, 7, 8, 14, 4, 2, N'Alumno falla continuamente', N'Prueba', CAST(N'2022-10-14T16:20:39.390' AS DateTime), NULL, NULL)
GO
INSERT [Students].[Notas] ([RowID], [EstudianteID], [Materia], [Jornada], [HorasSemana], [Nota], [Comentario], [UsuarioCreador], [FechaCreacion], [UsuarioModificador], [FechaModificacion]) VALUES (4, 7, 9, 14, 2, 4, N'Alumno distraido', N'Prueba', CAST(N'2022-10-14T16:22:55.517' AS DateTime), NULL, NULL)
GO
SET IDENTITY_INSERT [Students].[Notas] OFF
GO
/****** Object:  StoredProcedure [dbo].[SP_Estudiantes_INSERT]    Script Date: 14/10/2022 11:40:44 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
--Creado por: Juan Andrés Porras
--Fecha creación: 14/10/2022
--Descripción: Procedimiento para insertar un registro en la tabla [Students].[Estudiantes]
--Modificado por:
--Fecha modificación:
--____________________________________

CREATE PROCEDURE [dbo].[SP_Estudiantes_INSERT]

 @Nombre VARCHAR(50) = ''
,@Apellidos VARCHAR(50) = ''
,@Direccion VARCHAR(50) = ''
,@Grado INT = 0
,@Edad INT = 0
,@Sexo INT = 0
,@Comentario VARCHAR(400) = ''
AS
BEGIN
DECLARE @msg INT = NULL;
INSERT INTO [Students].[Estudiantes]
	   (
	   [Nombres]
      ,[Apellidos]
      ,[Direccion]
      ,[Grado]
      ,[Edad]
      ,[Sexo]
      ,[Comentario]
      ,[UsuarioCreador]
      ,[FechaCreacion]
	  )
VALUES
	  (
	   @Nombre
	  ,@Apellidos
	  ,@Direccion
	  ,@Grado
	  ,@Edad
	  ,@Sexo
	  ,@Comentario
	  ,'Prueba'
	  ,GETDATE()
	  )
	  	SET @msg = SCOPE_IDENTITY();
	
	IF(@msg IS NULL OR @msg  = 0)
	BEGIN
		SET @msg = @@IDENTITY
	END
	SELECT @msg AS [RowID]
END
GO
/****** Object:  StoredProcedure [dbo].[SP_Estudiantes_UPDATE]    Script Date: 14/10/2022 11:40:44 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
--Creado por: Juan Andrés Porras
--Fecha creación: 14/10/2022
--Descripción: Procedimiento para editar un registro en la tabla [Students].[Estudiantes]
--Modificado por:
--Fecha modificación:
--____________________________________

CREATE PROCEDURE [dbo].[SP_Estudiantes_UPDATE]

 @Nombre VARCHAR(50) = ''
,@Apellidos VARCHAR(50) = ''
,@Direccion VARCHAR(50) = ''
,@Grado INT = 0
,@Edad INT = 0
,@Sexo INT = 0
,@Comentario VARCHAR(400) = ''
,@RowID INT = 0
AS
BEGIN

UPDATE [Students].[Estudiantes] SET
	   [Nombres]				= @Nombre
      ,[Apellidos]				= @Apellidos
      ,[Direccion]				= @Direccion
      ,[Grado]					= @Grado
      ,[Edad]					= @Edad
      ,[Sexo]					= @Sexo
      ,[Comentario]				= @Comentario
      ,[UsuarioModificador]		= 'Prueba'
      ,[FechaModificacion]		= GETDATE()
WHERE [RowID] = @RowID

END
GO
/****** Object:  StoredProcedure [dbo].[SP_Notas_INSERT]    Script Date: 14/10/2022 11:40:44 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
--Creado por: Juan Andrés Porras
--Fecha creación: 14/10/2022
--Descripción: Procedimiento para insertar un registro en la tabla [Students].[Notas]
--Modificado por:
--Fecha modificación:
--____________________________________

CREATE PROCEDURE [dbo].[SP_Notas_INSERT]

 @EstudianteID INT = 0
,@MateriaID INT = 0
,@JornadaID INT = 0
,@HorasSemana INT = 0
,@Nota INT = 0
,@Comentario VARCHAR(400) = ''
AS
BEGIN
	DECLARE @msg INT = NULL;
INSERT INTO [Students].[Notas]
	  (
       [EstudianteID]
      ,[Materia]
      ,[Jornada]
      ,[HorasSemana]
      ,[Nota]
      ,[Comentario]
      ,[UsuarioCreador]
      ,[FechaCreacion]

	  )
VALUES
	  (
	   @EstudianteID
	  ,@MateriaID 
	  ,@JornadaID
	  ,@HorasSemana
	  ,@Nota
	  ,@Comentario
	  ,'Prueba'
	  ,GETDATE()
	  )
	  	  	SET @msg = SCOPE_IDENTITY();
	
	IF(@msg IS NULL OR @msg  = 0)
	BEGIN
		SET @msg = @@IDENTITY
	END
	SELECT @msg AS [RowID]
END
GO
/****** Object:  StoredProcedure [dbo].[SP_Notas_UPDATE]    Script Date: 14/10/2022 11:40:44 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
--Creado por: Juan Andrés Porras
--Fecha creación: 14/10/2022
--Descripción: Procedimiento para edita un registro en la tabla [Students].[Notas]
--Modificado por:
--Fecha modificación:
--____________________________________

CREATE PROCEDURE [dbo].[SP_Notas_UPDATE]

 @EstudianteID INT = 0
,@MateriaID INT = 0
,@JornadaID INT = 0
,@HorasSemana INT = 0
,@Nota INT = 0
,@Comentario VARCHAR(400) = ''
,@RowID INT = 0
AS
BEGIN

UPDATE [Students].[Notas] SET
       [EstudianteID]			= @EstudianteID
      ,[Materia]				= @MateriaID 
      ,[Jornada]				= @JornadaID
      ,[HorasSemana]			= @HorasSemana
      ,[Nota]					= @Nota
      ,[Comentario]				= @Comentario
      ,[UsuarioModificador]		= 'Prueba'
      ,[FechaModificacion]		= GETDATE()
WHERE [RowID] = @RowID

END
GO
/****** Object:  StoredProcedure [Students].[SP_Estudiante_DELETE]    Script Date: 14/10/2022 11:40:44 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
--Creado por: Juan Andrés Porras
--Fecha creación: 14/10/2022
--Descripción: Procedimiento para eliminar un registro en la tabla [Students].[Notas]
--Modificado por:
--Fecha modificación:
--____________________________________

CREATE PROCEDURE [Students].[SP_Estudiante_DELETE]

@RowID INT = 0
AS
BEGIN

DELETE FROM [Students].[Estudiantes] WHERE RowID =@RowID

END
GO
/****** Object:  StoredProcedure [Students].[SP_Estudiantes_Table]    Script Date: 14/10/2022 11:40:44 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO


--Creado por: Juan Andrés Porras
--Fecha creación: 14/10/2022
--Descripción: Procedimiento para Listar estudiantes
--Modificado por:
--Fecha modificación:
--____________________________________
/*
	EXEC [Students].[SP_Estudiantes_Table]
*/
CREATE PROCEDURE [Students].[SP_Estudiantes_Table]

AS
BEGIN
SELECT 
      [Master].[Fn_GetMenuStandard]([RowID],'Estudiantes') AS html
	  ,[RowID]
      ,[Nombres]
      ,[Apellidos]
      ,[Direccion]
      ,[GradoID]
      ,[GradoName]
      ,[Edad]
      ,[SexoID]
      ,[SexoName]
      ,[Comentario]
      ,[UsuarioCreador]
      ,[FechaCreacion]
      ,[UsuarioModificador]
      ,[FechaModificacion]
  FROM [Students].[vw_Estudiantes_List]
END
GO
/****** Object:  StoredProcedure [Students].[SP_Notas_Table]    Script Date: 14/10/2022 11:40:44 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO


--Creado por: Juan Andrés Porras
--Fecha creación: 14/10/2022
--Descripción: Procedimiento para Listar Notas(Materias)
--Modificado por:
--Fecha modificación:
--____________________________________
/*
	EXEC [Students].[SP_Notas_Table]

*/
CREATE PROCEDURE [Students].[SP_Notas_Table]

@RowID INT = 0
AS
BEGIN
SELECT 
       [Master].[Fn_GetMenuStandard]([RowID],'Notas') AS html
	  ,[RowID]
      ,[EstudianteID]
      ,[MateriaID]
      ,[MateriaName]
      ,[JornadaID]
      ,[JornadaName]
      ,[HorasSemana]
      ,[Nota]
      ,[Comentario]
      ,[UsuarioCreador]
      ,[FechaCreacion]
      ,[UsuarioModificador]
      ,[FechaModificacion]
  FROM [Students].[vw_Notas_List]
	WHERE [EstudianteID] = @RowID
END
GO
USE [master]
GO
ALTER DATABASE [Estudiantes_Santel] SET  READ_WRITE 
GO
