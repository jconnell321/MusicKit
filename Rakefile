desc "Generate Chord.swift and ChordQualities.swift"
task :codegen do
	Rake::Task['gen_chordqualities'].execute
	Rake::Task['gen_chord'].execute
end

task :docgen do
	Rake::Task['gen_frameworkoverview'].execute
end

task :gen_chordqualities do
	infile = "MusicKit/ChordQuality.swift"
	outfile = "MusicKit/ChordQualities.swift"
	lines = File.readlines(infile)

	newlines = [
		"import Foundation", "\n",
		"public struct ChordQualities {",
	]

	starts = "//:"
	continues = "//>"
	ends = "//."

	all_qualities = []
	for line in lines
		if line.include? starts
			group = line.sub(starts, "").strip.capitalize
			newlines.push "public static let #{group} = ["
		end
		if line.include? continues
			group = line.sub(continues, "").strip.capitalize
			newlines.push "]"
			newlines.push "public static let #{group} = ["
		end 
		if line.include? ends
			newlines.push "]"
			newlines.push "public static let All = ["
			newlines.concat all_qualities
			newlines.push "]"
			newlines.push "}"
			break
		end 
		if line.include? "case"
			quality = line.rpartition('=')[0].sub("case", "").strip
			qline = "ChordQuality.#{quality},"
			all_qualities.push qline
			newlines.push qline
		end
	end

	File.open(outfile, "w").puts newlines
end

task :gen_chord do
	infile = "MusicKit/ChordQuality.swift"
	outfile = "MusicKit/Chord.swift"
	lines = File.readlines(infile)

	newlines = [
		"import Foundation", "\n",
		"public enum Chord  {",
	]

	starts = "//:"
	continues = "//>"
	ends = "//."

	for line in lines
		if line.include? starts
			newlines.push line.strip
		end
		if line.include? continues
			newlines.push line.strip
		end 
		if line.include? ends
			newlines.push "}"
			break
		end 
		if line.include? "case"
			quality = line.rpartition('=')[0].sub("case", "").strip
			newlines.push "public static let #{quality} = Harmony.create(ChordQuality.#{quality}.intervals)"
		end
	end

	File.open(outfile, "w").puts newlines
end

task :gen_frameworkoverview do
	infile = "MusicKit.playground/section-1.swift"
	outfile = "Documentation/FrameworkOverview.md"
	lines = File.readlines(infile).drop(3).join("").gsub("///", "")
	File.open(outfile, "w").puts lines
end