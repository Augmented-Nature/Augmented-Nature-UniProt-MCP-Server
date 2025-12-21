[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/augmented-nature-augmented-nature-uniprot-mcp-server-badge.png)](https://mseep.ai/app/augmented-nature-augmented-nature-uniprot-mcp-server)


# Unofficial UniProt MCP Server

A comprehensive Model Context Protocol (MCP) server providing advanced access to the UniProt protein database. This server offers 26 specialized bioinformatics tools enabling AI assistants and MCP clients to perform sophisticated protein research, comparative genomics, structural biology analysis, and systems biology investigations directly through UniProt's REST API.

**Developed by [Augmented Nature](https://augmentednature.ai)**

## Features

### **Core Protein Analysis (5 tools)**

- **Protein Search**: Search the UniProt database by protein name, keywords, or organism
- **Detailed Protein Info**: Retrieve comprehensive protein information including function, structure, and annotations
- **Gene-based Search**: Find proteins by gene name or symbol
- **Sequence Retrieval**: Get amino acid sequences in FASTA or JSON format
- **Feature Analysis**: Access functional domains, active sites, binding sites, and other protein features

### **Comparative & Evolutionary Analysis (4 tools)**

- **Protein Comparison**: Side-by-side comparison of multiple proteins with sequence and feature analysis
- **Homolog Discovery**: Find homologous proteins across different species
- **Ortholog Identification**: Identify orthologous proteins for evolutionary studies
- **Phylogenetic Analysis**: Retrieve evolutionary relationships and phylogenetic data

### **Structure & Function Analysis (4 tools)**

- **3D Structure Information**: Access PDB references and structural data
- **Advanced Domain Analysis**: Enhanced domain analysis with InterPro, Pfam, and SMART annotations
- **Variant Analysis**: Disease-associated variants and mutations
- **Sequence Composition**: Amino acid composition, hydrophobicity, and other sequence properties

### **Biological Context Analysis (4 tools)**

- **Pathway Integration**: Associated biological pathways from KEGG and Reactome
- **Protein Interactions**: Protein-protein interaction networks
- **Functional Classification**: Search by GO terms or functional annotations
- **Subcellular Localization**: Find proteins by subcellular localization

### **Batch Processing & Advanced Search (3 tools)**

- **Batch Processing**: Efficiently process multiple protein accessions
- **Advanced Search**: Complex queries with multiple filters (length, mass, organism, function)
- **Taxonomic Classification**: Search by detailed taxonomic classification

### **Literature & Cross-References (3 tools)**

- **External Database Links**: Links to PDB, EMBL, RefSeq, Ensembl, and other databases
- **Literature References**: Associated publications and citations
- **Annotation Quality**: Quality scores and confidence levels for different annotations

### **Data Export & Utilities (3 tools)**

- **Specialized Export**: Export data in GFF, GenBank, EMBL, and XML formats
- **Accession Validation**: Verify UniProt accession number validity
- **Taxonomic Information**: Detailed taxonomic classification and lineage data

### **Resource Templates**

- Direct access to protein data via URI templates for seamless integration

## Installation

### Prerequisites

- Node.js (v16 or higher)
- npm or yarn

### Setup

1. Clone the repository:

```bash
git clone <repository-url>
cd uniprot-server
```

2. Install dependencies:

```bash
npm install
```

3. Build the project:

```bash
npm run build
```

## Docker

### Building the Docker Image

Build the Docker image:

```bash
docker build -t uniprot-mcp-server .
```

### Running with Docker

Run the container:

```bash
docker run -i uniprot-mcp-server
```

For MCP client integration, you can use the container directly:

```json
{
  "mcpServers": {
    "uniprot": {
      "command": "docker",
      "args": ["run", "-i", "uniprot-mcp-server"],
      "env": {}
    }
  }
}
```

### Docker Compose (Optional)

Create a `docker-compose.yml` for easier management:

```yaml
version: "3.8"
services:
  uniprot-mcp:
    build: .
    image: uniprot-mcp-server
    stdin_open: true
    tty: true
```

Run with:

```bash
docker-compose up
```

## Usage

### As an MCP Server

The server is designed to run as an MCP server that communicates via stdio:

```bash
npm start
```

### Adding to MCP Client Configuration

Add the server to your MCP client configuration (e.g., Claude Desktop):

```json
{
  "mcpServers": {
    "uniprot": {
      "command": "node",
      "args": ["/path/to/uniprot-server/build/index.js"],
      "env": {}
    }
  }
}
```

## Available Tools

### 1. search_proteins

Search the UniProt database for proteins by name, keyword, or organism.

**Parameters:**

- `query` (required): Search query (protein name, keyword, or complex search)
- `organism` (optional): Organism name or taxonomy ID to filter results
- `size` (optional): Number of results to return (1-500, default: 25)
- `format` (optional): Output format - json, tsv, fasta, xml (default: json)

**Example:**

```javascript
{
  "query": "insulin",
  "organism": "human",
  "size": 5
}
```

### 2. get_protein_info

Get detailed information for a specific protein by UniProt accession.

**Parameters:**

- `accession` (required): UniProt accession number (e.g., P04637)
- `format` (optional): Output format - json, tsv, fasta, xml (default: json)

**Example:**

```javascript
{
  "accession": "P01308",
  "format": "json"
}
```

### 3. search_by_gene

Search for proteins by gene name or symbol.

**Parameters:**

- `gene` (required): Gene name or symbol (e.g., BRCA1, INS)
- `organism` (optional): Organism name or taxonomy ID to filter results
- `size` (optional): Number of results to return (1-500, default: 25)

**Example:**

```javascript
{
  "gene": "BRCA1",
  "organism": "human"
}
```

### 4. get_protein_sequence

Get the amino acid sequence for a protein.

**Parameters:**

- `accession` (required): UniProt accession number
- `format` (optional): Output format - fasta, json (default: fasta)

**Example:**

```javascript
{
  "accession": "P01308",
  "format": "fasta"
}
```

### 5. get_protein_features

Get functional features and domains for a protein.

**Parameters:**

- `accession` (required): UniProt accession number

**Example:**

```javascript
{
  "accession": "P01308"
}
```

## Resource Templates

The server provides direct access to UniProt data through URI templates:

### 1. Protein Information

- **URI**: `uniprot://protein/{accession}`
- **Description**: Complete protein information for a UniProt accession
- **Example**: `uniprot://protein/P01308`

### 2. Protein Sequence

- **URI**: `uniprot://sequence/{accession}`
- **Description**: FASTA format protein sequence
- **Example**: `uniprot://sequence/P01308`

### 3. Search Results

- **URI**: `uniprot://search/{query}`
- **Description**: Search results for proteins matching the query
- **Example**: `uniprot://search/insulin`

## Examples

### Basic Protein Search

Search for insulin proteins in humans:

```javascript
// Tool call
{
  "tool": "search_proteins",
  "arguments": {
    "query": "insulin",
    "organism": "human",
    "size": 10
  }
}
```

### Get Detailed Protein Information

Retrieve comprehensive information about human insulin:

```javascript
// Tool call
{
  "tool": "get_protein_info",
  "arguments": {
    "accession": "P01308"
  }
}
```

### Gene-based Search

Find proteins associated with the BRCA1 gene:

```javascript
// Tool call
{
  "tool": "search_by_gene",
  "arguments": {
    "gene": "BRCA1",
    "organism": "human"
  }
}
```

### Retrieve Protein Sequence

Get the amino acid sequence for human insulin:

```javascript
// Tool call
{
  "tool": "get_protein_sequence",
  "arguments": {
    "accession": "P01308",
    "format": "fasta"
  }
}
```

### Analyze Protein Features

Get functional domains and features for human insulin:

```javascript
// Tool call
{
  "tool": "get_protein_features",
  "arguments": {
    "accession": "P01308"
  }
}
```

## API Integration

This server integrates with the UniProt REST API for programmatic access to protein data. For more information about UniProt:

- **UniProt Website**: https://www.uniprot.org/
- **API Documentation**: https://www.uniprot.org/help/api
- **REST API Guide**: https://www.uniprot.org/help/api_queries

All API requests include:

- **User-Agent**: `UniProt-MCP-Server/1.0.0`
- **Timeout**: 30 seconds
- **Base URL**: `https://rest.uniprot.org` (programmatic access only)

## Error Handling

The server includes comprehensive error handling:

- **Input Validation**: All parameters are validated using type guards
- **API Errors**: Network and API errors are caught and returned with descriptive messages
- **Timeout Handling**: Requests timeout after 30 seconds
- **Graceful Degradation**: Partial failures are handled appropriately

## Development

### Build the Project

```bash
npm run build
```

### Development Mode

Run TypeScript compiler in watch mode:

```bash
npm run dev
```

### Project Structure

```
uniprot-server/
├── src/
│   └── index.ts          # Main server implementation
├── build/                # Compiled JavaScript output
├── package.json          # Node.js dependencies and scripts
├── tsconfig.json         # TypeScript configuration
└── README.md            # This file
```

## Dependencies

- **@modelcontextprotocol/sdk**: Core MCP SDK for server implementation
- **axios**: HTTP client for UniProt API requests
- **typescript**: TypeScript compiler for development

## License

MIT License

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Support

For issues and questions:

1. Check the [UniProt API documentation](https://www.uniprot.org/help/api)
2. Review the [Model Context Protocol specification](https://modelcontextprotocol.io/)
3. Open an issue on the repository

## About Augmented Nature

This comprehensive UniProt MCP Server is developed by **[Augmented Nature](https://augmentednature.ai)**, a leading innovator in AI-powered bioinformatics and computational biology solutions. Augmented Nature specializes in creating advanced tools that bridge the gap between artificial intelligence and biological research, enabling researchers to unlock deeper insights from biological data.

## Complete Tool Reference

### **Core Protein Analysis Tools**

1. `search_proteins` - Search UniProt database by name, keyword, or organism
2. `get_protein_info` - Get detailed protein information by accession
3. `search_by_gene` - Find proteins by gene name or symbol
4. `get_protein_sequence` - Retrieve amino acid sequences
5. `get_protein_features` - Access functional features and domains

### **Comparative & Evolutionary Analysis Tools**

6. `compare_proteins` - Compare multiple proteins side-by-side
7. `get_protein_homologs` - Find homologous proteins across species
8. `get_protein_orthologs` - Identify orthologous proteins
9. `get_phylogenetic_info` - Retrieve evolutionary relationships

### **Structure & Function Analysis Tools**

10. `get_protein_structure` - Access 3D structure information from PDB
11. `get_protein_domains_detailed` - Enhanced domain analysis (InterPro, Pfam, SMART)
12. `get_protein_variants` - Disease-associated variants and mutations
13. `analyze_sequence_composition` - Amino acid composition analysis

### **Biological Context Tools**

14. `get_protein_pathways` - Associated biological pathways (KEGG, Reactome)
15. `get_protein_interactions` - Protein-protein interaction networks
16. `search_by_function` - Search by GO terms or functional annotations
17. `search_by_localization` - Find proteins by subcellular localization

### **Batch Processing & Advanced Search Tools**

18. `batch_protein_lookup` - Process multiple accessions efficiently
19. `advanced_search` - Complex queries with multiple filters
20. `search_by_taxonomy` - Search by taxonomic classification

### **Literature & Cross-Reference Tools**

21. `get_external_references` - Links to other databases (PDB, EMBL, RefSeq, etc.)
22. `get_literature_references` - Associated publications and citations
23. `get_annotation_confidence` - Quality scores for annotations

### **Data Export & Utility Tools**

24. `export_protein_data` - Export in specialized formats (GFF, GenBank, EMBL, XML)
25. `validate_accession` - Check accession number validity
26. `get_taxonomy_info` - Detailed taxonomic information

## Changelog

### v1.0.0 - Comprehensive Bioinformatics Platform

- **Major expansion**: Added 21 new specialized tools (total: 26 tools)
- **Comparative Analysis**: Protein comparison, homolog/ortholog identification, phylogenetic analysis
- **Structural Biology**: 3D structure integration, detailed domain analysis, variant analysis
- **Systems Biology**: Pathway integration, protein interactions, functional classification
- **Advanced Search**: Batch processing, complex filtering, taxonomic search
- **Literature Integration**: External database links, citations, annotation confidence
- **Data Export**: Multiple specialized formats (GFF, GenBank, EMBL, XML)
- **Enhanced Docker Support**: Multi-stage builds with security best practices
- **Comprehensive Documentation**: Complete tool reference and examples
- **Developed by Augmented Nature**: Professional bioinformatics platform

## Citation
If you use this project in your research or publications, please cite it as follows:

```bibtex @misc{uniprotmcp2025, 
author = {Moudather Chelbi},
title = {UniProt MCP Server},
year = {2025},
howpublished = {https://github.com/Augmented-Nature/Augmented-Nature-UniProt-MCP-Server/},
note = {Accessed: 2025-06-29}
