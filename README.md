# Covid-Fatality-and-vaccination-Analysis
● Used SQL Server Management to write queries, used concept of different joins, implemented relationship between tables.
● Likelihood of dying if you contact covid is maximum in Europe with death percentage as 17.12%.
● Highest Infection Count is 44Million in Europe.
● North America has highest number of vaccinations for the population.

Select * From [Portfolio project]..CovidDeaths
order by 3 ,4

--Select *
--From [Portfolio project]..CovidVaccination
--order by 3 ,4

--Select data that we are going to use

Select Location, date, total_cases, new_cases, total_deaths, population
From [Portfolio project]..CovidDeaths
order by 1,2

--Looking at Total Cases vs Total Deaths
--Shows likelihood of dying if you contact covid in your country
Select Location, date, total_cases,total_deaths, (total_deaths/total_cases)*100 as Deathpercentage
From [Portfolio project]..CovidDeaths
Where Location like '%states%'
order by 1,2

--Looking at Total cases vs population
--shows what percentage of population got covid
Select Location, date, total_cases,population,(total_cases/population)*100 as Deathpercentage
From [Portfolio project]..CovidDeaths
Where Location like '%states%'
order by 1,2

--Looking at countries with highest infection rate compared to population
Select Location, Population, MAX(total_cases) as HighestInfectionCount,  Max((total_cases/population))*100 as PercentPopulationInfected
From [Portfolio project]..CovidDeaths
--Where location like '%states%'
Group by Location, Population
order by PercentPopulationInfected desc

-- Countries with Highest Death Count per Population

Select Location, MAX(cast(total_deaths as int)) as TotalDeathCount
From PortfolioProject..CovidDeaths
--Where location like '%states%'
Where continent is not null 
Group by Location
order by TotalDeathCount desc



-- BREAKING THINGS DOWN BY CONTINENT

-- Showing contintents with the highest death count per population

Select continent, MAX(cast(Total_deaths as int)) as TotalDeathCount
From PortfolioProject..CovidDeaths
--Where location like '%states%'
Where continent is not null 
Group by continent
order by TotalDeathCount desc



-- GLOBAL NUMBERS
Select date, SUM(new_cases) as total_cases, SUM(cast(new_deaths as int)) as total_deaths, SUM(cast(new_deaths as int))/SUM((new_cases))*100 as Deathpercentage
From [Portfolio project]..CovidDeaths
where continent is not null
Group By date  
order by 1,2




Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
From [Portfolio project]..CovidDeaths dea
Join [Portfolio project]..CovidVaccination vac
On dea.location=vac.location
and dea.date=vac.date
where dea.continent is not null
order by 2,3
